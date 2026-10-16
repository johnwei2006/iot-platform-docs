# 11 · 业务需求（Requirements）

本目录承载平台的**业务需求与功能需求**，是后续设计、测试、部署、运维的基线。

## 目录索引

| 文件 | 说明 |
| --- | --- |
| [business-requirements.md](business-requirements.md) | 平台总体业务需求：设备接入、管理、监控、运营 |
| [ProxyTcp/requirements.md](ProxyTcp/requirements.md) | TCP 接入网关需求：接入、编解码、持久化、转发 |
| [platform_java/requirements.md](platform_java/requirements.md) | 后台业务平台需求：设备管理、业务 API |
| [dtm/requirements.md](dtm/requirements.md) | 数据传输服务需求：汇聚、转发、削峰 |
| [web_vue3/requirements.md](web_vue3/requirements.md) | 管理前端需求：设备监控、配置、报表 |
| [mini_uniapp/requirements.md](mini_uniapp/requirements.md) | 小程序需求：设备状态、轻量交互 |

## 需求编号规范

- 平台级需求：`BR-xxx`（Business Requirement）。
- 组件级功能需求：`FR-<组件>-xxx`（Functional Requirement）。
- 非功能需求：`NFR-xxx`。

## 关联文档

- 上游：无（需求为起点）。
- 下游：[`../12_design/`](../12_design/README.md)（基于需求开展设计）。
- 规范：[`../doc-conventions.md`](../doc-conventions.md)。
