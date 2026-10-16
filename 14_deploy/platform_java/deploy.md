# platform_java 部署手册（后台业务平台）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：DevOps Agent

## 1. 环境依赖

- JDK（建议 8/11，与 `ProxyTcp` 协调）。
- 关系数据库 + MongoDB（业务/设备数据）。
- RabbitMQ（与 `dtm` 对接下行）。
- Redis（缓存/会话，建议）。

## 2. 构建

```bash
mvn clean package        # 产物 platform-java.jar
# 或容器化构建（见 deploy-architecture.md）
```

## 3. 运行

```bash
java -jar -Xms512M -Xmx2048M platform-java.jar
```

- 配置：外挂 `application.yml` / `config/`。
- 启动后：消费 `dtm` 上行、提供 REST API、发布下行到 MQ。

## 4. 高可用

- 多实例无状态部署，前置负载均衡。
- 数据库主从 / 集群。

## 5. 验证

| 项 | 方法 | 期望 |
| --- | --- | --- |
| 健康检查 | `/actuator/health`（建议启用） | UP |
| API | 调用登录/设备列表 | 正常返回 |
| 上行 | 经 dtm 发上行 | 数据落库 |
| 下行 | 调下发接口 | 指令进入 MQ |

## 6. 回滚

- 保留上一版本 jar，重启回滚；数据库变更需兼容（向前兼容迁移）。
