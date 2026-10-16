# web_vue3 部署手册（管理前端）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：DevOps Agent

## 1. 环境依赖

- Node.js >= 22.12.0（Vite 8 要求）。
- npm（或 pnpm / yarn）。

## 2. 构建

```bash
npm install
npm run build      # 产物 dist/
```

- 环境变量（`VITE_` 前缀）注入后台 API 基地址等。

## 3. 托管

静态资源托管方式任选：

- **Nginx**：`root` 指向 `dist/`，配置 SPA 回退（`try_files $uri /index.html`）。
- **对象存储 / CDN**：上传 `dist/` 并配置默认文档与历史回退。

Nginx 示例：

```nginx
server {
  listen 80;
  root /var/www/web_vue3/dist;
  location / { try_files $uri /index.html; }
}
```

## 4. 验证

- 访问站点，登录后各页面加载正常。
- 浏览器控制台无错误，API 联通。

## 5. 回滚

- 保留上一版本 `dist/`，切换软链或重新发布。
- 前端改动需与后台 API 版本兼容。
