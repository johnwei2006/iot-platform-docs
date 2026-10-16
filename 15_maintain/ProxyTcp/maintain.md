# ProxyTcp 运维要点（TCP 接入网关）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：运维 Agent

## 1. 关键指标

- 在线设备连接数、上下行速率/时延、下行失败率、重发/死信数、JVM 内存与 GC。

## 2. 日志与排查

- 日志框架：log4j（见 `pom.xml`）。关注连接建立/断开、解码失败、下行失败日志。
- 解码失败：检查报文是否符合 `../12_design/message-protocol.md`。
- 下行失败：查 `SendFailureMessage` 错误码（离线/多次失败/未知设备/编码错误/其他）。

## 3. 配置项

- `proxy.properties`：RabbitMQ（`RABBITMQ_*`）、DB 类型（`dbtype=h2|mongo`）、
  去重窗口（`duplicateMessageDurationMinutes`）、在线判定（`onlineCheckJudgeMinutes`）、
  失败判定（`judgeSendFailedAfterMinutes`）。

## 4. 常见故障

| 故障 | 处置 |
| --- | --- |
| 连接数异常 | 查网络/资源；扩实例；防重连风暴 |
| 上行不落库 | 查 DB 连接与 `UplinkMessageService` |
| 下行不送达 | 查设备在线与失败消息 |
| MQ 不通 | 查 RabbitMQ 管理与网络 |

## 5. 备份与回滚

- 保留上一 `proxy.jar` 与 `config/proxy/`（见 `../14_deploy/ProxyTcp/deploy.md`）。
