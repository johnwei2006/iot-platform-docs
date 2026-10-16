# mini_uniapp 部署手册（微信小程序）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：DevOps Agent

## 1. 环境依赖

- Node.js（uni-app 构建）。
- 微信开发者工具（发布）。
- 小程序 AppID（替换 `manifest.json` 中 `mp-weixin.appid` 默认 `touristappid`）。

## 2. 构建

```bash
npm install
npm run build:mp-weixin   # 产物 dist/build/mp-weixin
```

## 3. 发布流程

1. 微信开发者工具导入 `dist/build/mp-weixin`。
2. 配置 AppID、服务器域名（后台 API 需 HTTPS 且加入白名单）。
3. 上传代码 → 微信公众平台提交审核 → 发布。

## 4. H5（可选）

```bash
npm run build:h5         # 产物 dist/build/h5
```
静态托管（同 `web_vue3/deploy.md`）。

## 5. 验证

- 真机/模拟器预览，授权登录、设备查看、轻量交互正常。
- 后台 API 域名白名单生效。

## 6. 回滚

- 微信平台可回退到已发布版本；保留历史构建产物。
