# 12 · 设计开发（Design）

本目录承载平台的**总体架构、报文协议、数据模型、接口契约与组件详细设计**。
其中架构、协议、模型、接口为**集中定义（单一事实来源）**，各组件设计仅描述自身并引用之。

## 目录索引

| 文件 | 说明 |
| --- | --- |
| [architecture.md](architecture.md) | 总体技术架构与分层 |
| [message-protocol.md](message-protocol.md) | 设备 TCP 报文协议（事实来源：`ProxyTcp/报文结构.pptx` 与 `codec` 实现） |
| [data-model.md](data-model.md) | 核心数据模型：设备 / 消息 / 网关 |
| [api-design.md](api-design.md) | 组件间与对外接口契约 |
| [ProxyTcp/design.md](ProxyTcp/design.md) | TCP 接入网关详细设计 |
| [platform_java/design.md](platform_java/design.md) | 后台业务平台详细设计 |
| [dtm/design.md](dtm/design.md) | 数据传输服务详细设计 |
| [web_vue3/design.md](web_vue3/design.md) | 管理前端详细设计 |
| [mini_uniapp/design.md](mini_uniapp/design.md) | 微信小程序详细设计 |

## 设计原则

- **集中定义、分散引用**：协议/模型/接口集中定义，组件设计引用而非重写。
- **事实来源优先**：报文协议以 `ProxyTcp/报文结构.pptx` 与代码 `codec/` 为准。
- **图即代码**：架构/流程/时序一律 Mermaid。

## 关联文档

- 上游：[`../11_requirements/`](../11_requirements/README.md)。
- 下游：[`../13_test/`](../13_test/README.md)、[`../14_deploy/`](../14_deploy/README.md)、[`../15_maintain/`](../15_maintain/README.md)。
- 规范：[`../doc-conventions.md`](../doc-conventions.md)。
