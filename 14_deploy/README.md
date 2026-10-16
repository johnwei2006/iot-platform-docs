# 14 · 部署（Deploy）

本目录承载平台的**部署架构与组件部署手册**，覆盖构建、部署、回滚与多环境管理。

## 目录索引

| 文件 | 说明 |
| --- | --- |
| [deploy-architecture.md](deploy-architecture.md) | 部署拓扑：网关 / 后台 / 前端 / 中间件 |
| [ProxyTcp/deploy.md](ProxyTcp/deploy.md) | TCP 网关部署手册（jar / 容器） |
| [platform_java/deploy.md](platform_java/deploy.md) | 后台部署手册 |
| [dtm/deploy.md](dtm/deploy.md) | 传输服务部署手册 |
| [web_vue3/deploy.md](web_vue3/deploy.md) | 前端构建与静态托管 |
| [mini_uniapp/deploy.md](mini_uniapp/deploy.md) | 小程序发布流程 |

## 环境约定

| 环境 | 用途 | 数据 |
| --- | --- | --- |
| dev | 开发自测 | 独立、可脏 |
| test | 集成与回归 | 接近生产 |
| prod | 生产 | 严格管控、备份 |

## 关联文档

- 上游：[`../12_design/`](../12_design/README.md)。
- 下游：[`../15_maintain/`](../15_maintain/README.md)。
- 规范：[`../doc-conventions.md`](../doc-conventions.md)。
