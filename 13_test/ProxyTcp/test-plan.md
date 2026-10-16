# ProxyTcp 测试计划（TCP 接入网关）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：测试 Agent

## 1. 测试范围

覆盖 `ProxyTcp` 接入、编解码、上下行、持久化、转发、重发、定时任务。

## 2. 单元测试

| 编号 | 对象 | 用例 |
| --- | --- | --- |
| TC-PX-U01 | `codec/v2/TransportcoderByteImplV2` | V2 字节解码/编码往返一致性（41 字节布局） |
| TC-PX-U02 | `codec/Message.toByteArray` | 四段拼接正确 |
| TC-PX-U03 | `utils/ByteUtil` | 大端 int↔byte 互转 |
| TC-PX-U04 | `utils/MD5Util` / `ShowFlakeIdGanerator` | 摘要与 ID 唯一性 |
| TC-PX-U05 | `Constants` 枚举 | 服务类型/编码/标识映射正确 |

## 3. 集成测试

| 编号 | 对象 | 用例 |
| --- | --- | --- |
| TC-PX-I01 | `TransportUplinkProcessor` | 上行消息解码→持久化→MQ 发布 |
| TC-PX-I02 | `DownlinkMessageService`（H2/Mongo） | 下行落库与状态流转 |
| TC-PX-I03 | RabbitMQ 收发 | 上行发布可被消费；下行可投递 |
| TC-PX-I04 | `DeviceInfoService` | 设备状态/连接信息更新 |

## 4. 端到端（设备模拟器）

| 编号 | 场景 | 期望 |
| --- | --- | --- |
| TC-PX-E01 | 模拟设备建连 + 心跳 | 在线状态正确 |
| TC-PX-E02 | 模拟上行报文 | 后台收到且持久化 |
| TC-PX-E03 | 平台下发指令（设备在线） | 设备收到下行 |
| TC-PX-E04 | 平台下发指令（设备离线） | 缓存，上线后补发 |
| TC-PX-E05 | 下行超重发上限 | 状态 `RESEND_OVERTIMES` |

## 5. 性能测试

| 指标 | 目标 | 方法 |
| --- | --- | --- |
| 并发长连接 | 单实例万级 | 设备模拟器并发建连 |
| 上行吞吐 | 满足接入速率 | 批量上行压测 |
| 上行时延 | P95 < 1s | 端到端打点 |

## 6. 测试报告模板

```
# ProxyTcp 测试报告 vX
- 环境：dev/test/prod
- 范围：单元/集成/端到端/性能
- 结果：通过 N / 失败 M（列表）
- 性能：并发/吞吐/时延
- 结论：可发布 / 阻塞项
```
