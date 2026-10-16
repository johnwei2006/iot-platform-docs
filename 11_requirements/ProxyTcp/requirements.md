# ProxyTcp 需求文档（TCP 接入网关）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：需求分析师 Agent（基于 `2_codes/ProxyTcp` 代码事实）

## 1. 背景与目标

`ProxyTcp` 是平台的**设备接入网关**，负责设备 TCP 长连接的接入、上下行报文编解码、
会话管理、RabbitMQ 转发与持久化。它是设备与平台之间的第一道入口。

> 本需求文档以现有 `ProxyTcp` 代码为事实来源（见 `Constants`、`ProxyController`、
> `codec/Message`、`transport/*`、`persistence/*`）。

## 2. 范围

### 含

- TCP 服务端监听与设备长连接管理（`tcp/server/TcpServer`、`ServerThread`）。
- 报文编解码：消息头 / 传输数据 / 业务数据 / 16 字节签名（`codec/Message`）。
- 传输版本（V1/V2）与编码类型（Byte/Protobuf/JSON）支持。
- 上行处理：解析、校验、去重、持久化、转发（`transport/TransportUplinkProcessor`）。
- 下行处理：指令编码、会话投递、失败重发（`transport/TransportDownlinkProcessor`、`ResendDownlinkProcessor`）。
- 服务类型：ACK、心跳（HEART_BEAT）、在线检测（ONLINE_CHECK）。
- 持久化：H2 / MongoDB 双实现（`persistence/h2`、`persistence/mongodb`）。
- RabbitMQ 上下行消息收发（`transport/amq`）。
- 管理 REST 接口（见 §4）。
- 设备信息、上下行消息、发送失败消息的查询与清理。

### 不含

- 复杂业务规则处理（归 `platform_java`）。
- 前端展示（归 `web_vue3` / `mini_uniapp`）。

## 3. 功能需求（FR）

| 编号 | 描述 | 优先级 | 验收标准 |
| --- | --- | --- | --- |
| FR-ProxyTcp-001 | TCP 端口监听并接受设备长连接 | P0 | 设备可建立并保持长连接 |
| FR-ProxyTcp-002 | 上行报文解码为业务对象（支持 V1/V2、Byte/Protobuf/JSON） | P0 | 各版本/编码报文正确解析 |
| FR-ProxyTcp-003 | 上行报文校验与签名验证（16 字节签名） | P0 | 非法签名报文被拒绝并记录 |
| FR-ProxyTcp-004 | 上行报文去重（按重复窗口 `duplicateMessageDurationMinutes`） | P1 | 窗口内重复报文被丢弃 |
| FR-ProxyTcp-005 | 上行报文持久化（上行消息、设备信息） | P0 | 消息落库可查 |
| FR-ProxyTcp-006 | 上行报文经 RabbitMQ 转发至 `dtm` | P0 | 消息可靠进入 MQ |
| FR-ProxyTcp-007 | 下行指令接收（`/downlink`）并编码投递到设备会话 | P0 | 设备在线时实时送达 |
| FR-ProxyTcp-008 | 下行失败处理：离线缓存、上线补发、重发上限（`RESEND_OVERTIMES`） | P0 | 失败消息进入重发，超限标记 FAILED |
| FR-ProxyTcp-009 | 设备心跳与在线状态检测（`ONLINE_CHECK`、在线判定分钟数） | P0 | 心跳超时判定离线 |
| FR-ProxyTcp-010 | 设备信息查询（VIN / 终端号 / 设备ID 三类标识） | P1 | 支持三种标识查询 |
| FR-ProxyTcp-011 | 发送失败消息管理（错误码：离线/多次失败/未知设备/编码错误/其他） | P1 | 失败原因可追溯 |
| FR-ProxyTcp-012 | 过期失败消息清理定时任务 | P2 | 按策略清理历史失败消息 |
| FR-ProxyTcp-013 | 下行重发接口（`/resendDownlink`） | P1 | 指定消息可手动重发 |
| FR-ProxyTcp-014 | 消息 ID 生成（雪花算法 `ShowFlakeIdGanerator`） | P0 | ID 全局唯一 |

## 4. 管理 REST 接口（现状）

| 接口 | 方法 / 路径 | 说明 |
| --- | --- | --- |
| 健康检查 | `GET /ping/{pong}` | 返回 pong，探活 |
| 下行指令 | `POST /downlink` | 接收下行 `ServiceRequest`，返回 `ServiceResponse` |
| 重发下行 | `POST /resendDownlink` | 按设备与报文字节重发 |
| 查询下行消息 | `GET /DownlinkMessages` | 返回所有下行消息 |
| 查询上行消息 | `GET /UplinkMessages` | 返回所有上行消息 |
| 查询设备信息 | `GET /DeviceInfos` | 返回所有设备信息 |
| 查询单设备 | `GET /DeviceInfos/{deviceId}` | 按设备标识查询 |

> 注：接口形态后续在 `../api-design.md` 统一规范（路径风格、统一响应体、鉴权）。

## 5. 非功能需求

| 编号 | 维度 | 目标 |
| --- | --- | --- |
| NFR-ProxyTcp-001 | 并发 | 单实例万级并发长连接 |
| NFR-ProxyTcp-002 | 可靠性 | 报文持久化 + 重发，不丢失 |
| NFR-ProxyTcp-003 | 存储兼容 | H2（轻量/嵌入式）与 MongoDB（生产）可切换（`dbtype`） |
| NFR-ProxyTcp-004 | 安全 | 接入鉴权、签名校验、传输可加密 |

## 6. 依赖与接口

- 下游：`dtm`（RabbitMQ 上行消费）、`platform_java`（下行指令来源）。
- 协议：`../message-protocol.md`。

## 7. 验收标准

- 上述 P0 功能均可通过设备模拟器验证。
- 上下行报文端到端不丢失，失败可重发、可查询。
