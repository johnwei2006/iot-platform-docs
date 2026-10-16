# ProxyTcp 部署手册（TCP 接入网关）

- 版本：v0.1
- 状态：草稿
- 最后更新：2026-10-16
- 作者：DevOps Agent
- 事实来源：`2_codes/ProxyTcp/java -jar.txt`、`mvn spring-boot.txt`、`pom.xml`

## 1. 环境依赖

- **JDK 8**：`set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_351`（生产用对应路径）。
- **RabbitMQ**：启用 management 插件，访问 `http://localhost:15672/`。
- **MongoDB 或 H2**：由 `Constants.DB_TYPE` 选择（`h2` / `mongo`）。
- 配置文件：`classpath:proxy.properties`（可外挂 `./config/proxy/`）。

## 2. 构建

```bash
# 开发运行（含远程调试，端口 5010）
mvn spring-boot:run -Drun.jvmArguments="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5010"

# 打包
mvn clean package
# 产物：./target/proxy.jar
```

## 3. 运行（jar）

```bash
java -Xbootclasspath/a:./config/proxy/ -jar -Xms512M -Xmx2048M ./target/proxy.jar
```

- `-Xbootclasspath/a:./config/proxy/`：外挂配置目录（优先于 classpath）。
- 启动后自动：上行处理 `TransportUplinkProcessor` + TCP 监听 `TcpServer`。

## 4. systemd 托管（Linux 生产）

`/etc/systemd/system/proxy.service` 示例：

```ini
[Unit]
Description=ProxyTcp IoT Gateway
After=network.target

[Service]
Type=simple
Environment=JAVA_HOME=/usr/lib/jvm/java-8-openjdk
WorkingDirectory=/opt/proxy
ExecStart=/usr/bin/java -Xbootclasspath/a:/opt/proxy/config/ -jar -Xms512M -Xmx2048M /opt/proxy/target/proxy.jar
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl restart proxy.service
```

## 5. 容器化（建议）

- 基础镜像：`openjdk:8-jre`。
- 暴露 TCP 监听端口与 HTTP 管理端口；挂载 `config/` 与日志卷。
- 详见 `../deploy-architecture.md`。

## 6. 验证

| 项 | 方法 | 期望 |
| --- | --- | --- |
| 探活 | `GET /ping/ok` | 返回 `ok` |
| 设备接入 | 设备模拟器建连 | 日志出现连接；`/DeviceInfos` 可查 |
| 上行 | 发送上行报文 | `/UplinkMessages` 出现记录 |
| 下行 | `POST /downlink` | 设备收到（或缓存） |

## 7. 回滚

- 保留上一版本 `proxy.jar`，重启服务即可回滚。
- 配置变更前备份 `config/proxy/`。
