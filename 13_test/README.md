# 13 · 测试（Test）

本目录承载平台的**测试策略与组件测试计划**，确保各组件与设计契约一致、质量可控。

## 目录索引

| 文件 | 说明 |
| --- | --- |
| [test-strategy.md](test-strategy.md) | 分层测试策略：单元 / 集成 / 端到端 |
| [ProxyTcp/test-plan.md](ProxyTcp/test-plan.md) | TCP 网关测试计划：编解码、会话、转发 |
| [platform_java/test-plan.md](platform_java/test-plan.md) | 后台测试计划：API、设备管理 |
| [dtm/test-plan.md](dtm/test-plan.md) | 传输服务测试计划：转发、削峰 |
| [web_vue3/test-plan.md](web_vue3/test-plan.md) | 前端测试计划：组件、接口 |
| [mini_uniapp/test-plan.md](mini_uniapp/test-plan.md) | 小程序测试计划：页面、交互 |

## 测试分层

| 层 | 目标 | 工具（建议） |
| --- | --- | --- |
| 单元测试 | 函数/类级正确性 | JUnit（Java）/ Vitest（前端） |
| 集成测试 | 组件内协作（DB、MQ） | Testcontainers / Spring Test |
| 端到端 | 跨组件数据流 | 设备模拟器 + 自动化脚本 |
| 性能测试 | 并发 / 时延 / 吞吐 | JMeter / 自研压测 |

## 关联文档

- 上游：[`../12_design/`](../12_design/README.md)（测试依据设计契约）。
- 下游：[`../14_deploy/`](../14_deploy/README.md)。
- 规范：[`../doc-conventions.md`](../doc-conventions.md)。
