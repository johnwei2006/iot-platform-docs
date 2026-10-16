# mini_uniapp 详细设计（微信小程序）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：架构师 Agent

> 脚手架约定见 `2_codes/mini_uniapp/README.md`；接口契约见 `../api-design.md` §5/§6。

## 1. 设计目标与约束

- 终端用户移动端：设备绑定、状态查看、轻量交互。
- 技术栈：uni-app（Vue3 组合式 API）+ TypeScript + Pinia + uni.request。
- 编译目标：微信小程序 / H5。

## 2. 目录结构（脚手架）

```
src/
├── api/         网络请求（uni.request 封装 + 业务接口）
├── components/  通用组件（uni 内置组件）
├── pages/       页面（index / login / mine）
├── store/       Pinia 状态（user 等模块）
├── types/       全局 TS 类型
├── utils/       工具（storage 等）
├── static/      静态资源
├── App.vue      应用入口（生命周期）
├── main.ts      入口（createSSRApp + Pinia）
├── pages.json   路由 / tabBar / 窗口配置
├── manifest.json 各端 appid 等
└── uni.scss     全局样式变量
```

## 3. 关键约定

- 使用 uni-app **内置组件**（`view`/`text`/`button`/`image`），不用 HTML 标签。
- 尺寸用 **rpx**（750rpx = 屏宽），多端自适应。
- 路由在 **pages.json** 声明，跳转用 `uni.navigateTo` / `switchTab` / `reLaunch`。
- 别名 `@` → `src`。

## 4. 页面规划

| 页面 | 说明 |
| --- | --- |
| 登录 | 微信授权登录 |
| 设备列表 | 绑定设备、状态 |
| 设备详情 | 在线/离线、最新数据 |
| 我的 | 用户信息、设置 |

## 5. 与后台契约

- 同 `../api-design.md` §5：统一 `ApiResponse`、Token 鉴权（微信登录换取平台 Token）。
- 网络层：`src/api/request.ts` 封装 `uni.request`，统一错误处理。

## 6. 发布

- 微信小程序：构建 `dist/build/mp-weixin` 后导入微信开发者工具上传。
- 详见 `../../14_deploy/mini_uniapp/deploy.md`。
