# 部署说明模板

> 用途：对外展示部署流程与技术成熟度。  
> 注意：本模板不包含真实密钥、真实 IP、真实账号密码。

---

## 1. 部署目标

- 完成服务端模块部署与启动
- 完成基础中间件连接验证
- 完成 API 可用性验证
- 完成管理后台可访问验证

---

## 2. 环境要求（示例）

## 2.1 服务器建议

- OS：Linux x86_64（Ubuntu/CentOS 任一）
- CPU：4 Core 及以上（生产建议 8 Core+）
- 内存：8GB 及以上（生产建议 16GB+）
- 磁盘：100GB 及以上 SSD

## 2.2 基础软件

- JDK 8
- MySQL 5.7+ / 8.x（按项目兼容性）
- Redis 5.x+
- RabbitMQ（如启用相关模块）
- ZooKeeper（如启用相关模块）
- Nginx（可选，反向代理）

---

## 3. 目录规划（示例）

```text
/opt/poker/
  ├─ services/
  │   ├─ login/
  │   ├─ web-api/
  │   ├─ web-message/
  │   ├─ calculator/
  │   ├─ pay/
  │   └─ ...
  ├─ config/
  │   ├─ application-develop.yml
  │   └─ ...
  ├─ logs/
  └─ scripts/
```

---

## 4. 配置文件模板（示例）

> 以下仅为示例字段

```yaml
server:
  port: 8088
  servlet:
    context-path: /api

spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/your_db?useUnicode=true&characterEncoding=UTF-8
    username: your_user
    password: your_password
  redis:
    host: 127.0.0.1
    port: 6379
    password: your_redis_password
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: your_mq_user
    password: your_mq_password

token:
  enable: true
  tokenHeaderName: md5at
  deviceHeaderName: did

swagger:
  enable: true
  title: Poker API
```

---

## 5. 构建与打包（示例）

## 5.1 单模块构建（示例：webapi）

```bash
cd game-01-club-jp/webapi
mvn clean package -DskipTests
```

## 5.2 Windows 一键打包（示例）

```powershell
cd game-01-club-jp
powershell -ExecutionPolicy Bypass -File .\scripts\release.ps1 -Service all -Profile aws-test -SkipTests
```

---

## 6. 启动步骤（示例）

1. 启动 MySQL / Redis / MQ / ZK
2. 导入数据库初始化脚本（按项目提供）
3. 检查各模块配置文件
4. 依赖顺序启动服务（示例）：
   - `login`
   - `web-api`
   - `web-message`
   - `calculator`
   - `pay`
   - 其他玩法服务与后台服务
5. 检查日志无报错

---

## 7. 验证清单（上线前）

- [ ] 管理后台可登录
- [ ] 关键 API 正常返回
- [ ] 创建俱乐部/加入俱乐部流程正常
- [ ] 支付回调（测试环境）可达
- [ ] 消息推送链路正常
- [ ] 日志无大量 ERROR
- [ ] 系统时间、时区一致

---

## 8. 常见问题排查

## 8.1 连接数据库失败

- 检查白名单、端口、账号权限
- 检查 JDBC 参数与字符集

## 8.2 Redis 连接失败

- 检查密码与防火墙
- 检查 Redis bind 与 protected-mode

## 8.3 MQ 消费异常

- 检查 exchange/queue/binding 是否创建
- 检查消费者配置与权限

## 8.4 服务启动后 404

- 检查 `context-path` 配置
- 检查 Nginx 转发路径

---

## 9. 安全建议

- 按环境分离配置（dev/test/prod）
- 开启最小权限访问策略
- 使用 HTTPS 与访问控制
- 建议开启审计日志与异常告警

---

## 10. 交付边界说明

- 本文档为通用部署示例，具体以项目交付版文档为准。
- 实际上线需结合客户基础设施进行参数调优。
- 第三方服务账号由客户侧提供并保管。

