# 15 · 运维（Maintain）

本目录承载平台的**监控告警体系与运维手册**，保障上线后的稳定性与可恢复性。

## 目录索引

| 文件 | 说明 |
| --- | --- |
| [monitoring.md](monitoring.md) | 监控告警体系：日志 / 指标 / 链路 |
| [ops-runbook.md](ops-runbook.md) | 通用运维手册与应急预案 |
| [ProxyTcp/maintain.md](ProxyTcp/maintain.md) | TCP 网关运维要点 |
| [platform_java/maintain.md](platform_java/maintain.md) | 后台运维要点 |
| [dtm/maintain.md](dtm/maintain.md) | 传输服务运维要点 |
| [web_vue3/maintain.md](web_vue3/maintain.md) | 前端运维要点 |
| [mini_uniapp/maintain.md](mini_uniapp/maintain.md) | 小程序运维要点 |

## 运维三支柱

1. **可观测**：日志、指标（Metrics）、链路追踪（Tracing）三位一体。
2. **可恢复**：明确回滚、故障转移、应急预案。
3. **可追溯**：变更、发布、故障均留痕。

## 关联文档

- 上游：[`../14_deploy/`](../14_deploy/README.md)。
- 规范：[`../doc-conventions.md`](../doc-conventions.md)。
