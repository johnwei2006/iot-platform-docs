# web_vue3 详细设计（管理前端）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：架构师 Agent

> 脚手架约定见 `2_codes/web_vue3/README.md`；接口契约见 `../api-design.md` §5/§6。

## 1. 设计目标与约束

- 运营人员控制台：设备监控、配置、报表、告警、系统管理。
- 技术栈：Vue3 + TypeScript + Pinia + Vue Router + Element Plus + Axios（Vite 构建）。
- 路径别名 `@` → `src`；响应结构 `ApiResponse`；环境变量 `VITE_` 前缀。

## 2. 目录结构（脚手架）

```
src/
├── api/          业务接口（Axios 封装）
├── components/   通用组件
├── layouts/      布局（含 Element Plus 布局）
├── plugins/      插件注册（Element Plus）
├── router/       路由与守卫
├── stores/       Pinia 状态（模块化）
├── styles/       全局样式
├── types/        全局 TS 类型（含 ApiResponse）
├── utils/        工具（storage 等）
├── views/        页面级视图
├── App.vue       根组件
└── main.ts       入口
```

## 3. 关键设计

- **API 层**：`src/api` 统一封装 Axios 拦截器（注入 Token、统一错误处理、适配 `ApiResponse`）。
- **状态管理**：Pinia 模块化（`stores/`），如 `user`、`device`。
- **路由守卫**：未登录跳转登录页；按角色控制菜单/权限。
- **布局**：`layouts/` 提供侧边栏 + 顶栏骨架，页面在 `views/`。

## 4. 页面规划

| 页面 | 说明 |
| --- | --- |
| 登录 | 鉴权获取 Token |
| 设备列表 | 分页、搜索、状态 |
| 设备详情 | 实时状态、最新数据、历史 |
| 配置下发 | 设备参数配置/指令 |
| 数据报表 | 统计图表 |
| 告警看板 | 实时告警与处理 |
| 系统管理 | 用户/角色/权限 |

## 5. 与后台契约

- 统一 `ApiResponse`：`{ code, message, data }`。
- 鉴权：`Authorization: Bearer <token>`。
- 接口资源见 `../api-design.md` §5。

## 6. 工程规范

- 严格 TypeScript；组件用 `<script setup>`；样式 BEM/Scoped。
- 构建：`npm run build` → `dist/`；托管见 `../../14_deploy/web_vue3/deploy.md`。
