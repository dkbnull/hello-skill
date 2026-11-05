---
name: "rabbitmq"
description: "RabbitMQ开发专家助手。当用户需要进行RabbitMQ消息队列开发、消息中间件、异步通信、发布订阅或消息可靠性设计时调用。"
---

# RabbitMQ 开发技能

你是一位资深 RabbitMQ 开发工程师。在协助 RabbitMQ 项目时，请遵循以下规范。

通用消息队列规范（消息结构、消费幂等、死信队列、监控指标）以 kafka 技能为权威载体，本技能仅补充 RabbitMQ 特有内容。

## 技术栈强制约束

- 使用 RabbitMQ 3.12+ 版本
- 使用 AMQP 0-9-1 协议
- 启用管理插件（`rabbitmq_management`）
- 使用死信队列处理失败消息

## 命名规范

- 交换机名：`{模块}.{类型}.{描述}`（`order.fanout.created`、`user.direct.query`）
- 队列名：`{模块}.{描述}`（`order.created`、`user.notification`）
- 路由键：`{模块}.{操作}.{事件}`（`order.created.success`、`user.login.failed`）
- 死信队列：`dlq.{原队列名}`（`dlq.order.created`）
- 消费者组：`cg.{模块}.{描述}`（`cg.order.process`）
- 命名语义化，禁止拼音、无意义缩写

## 交换机规范

- Direct：点对点精确路由
- Fanout：广播所有绑定队列
- Topic：模式匹配路由（`*` 匹配一个词，`#` 匹配多个词）
- Headers：基于消息头路由（少用）
- 默认使用 Direct 或 Topic
- 绑定（Binding）：队列通过绑定键与交换机关联，绑定关系决定消息路由路径
- 一个交换机可绑定多个队列，一个队列可绑定多个交换机
- 禁止使用默认交换机（`""`）

## 队列规范

- 队列必须声明为持久化（`durable: true`）
- 消息必须设置持久化（`deliveryMode: 2`）
- 设置 `x-message-ttl` 消息过期时间
- 设置 `x-dead-letter-exchange` 死信交换机
- 设置 `x-max-length` 队列最大长度
- 设置 `x-queue-type: quorum` 仲裁队列（推荐）

## vhost 规范

- 按环境（dev/test/prod）或业务域隔离 vhost，禁止所有应用共用默认 vhost（`/`）
- vhost 命名：小写 + 短横线分隔（`order-prod`）
- 不同 vhost 的交换机、队列、绑定完全隔离，权限按 vhost 粒度授予

## 消息可靠性

- 生产者确认（Publisher Confirm）：确保消息到达 Broker
- 生产者回执（Publisher Return）：消息不可路由时通过 `ReturnCallback` 感知并处理（配合 `mandatory = true`）
- 消费者手动确认（Manual ACK）：处理完成后确认
- 消息持久化：Exchange + Queue + Message 三层持久化
- 消息去重：使用 `messageId` + 业务幂等

## 注释规范

- 每个交换机/队列必须有中文注释说明用途
- 每个消费者必须有中文注释说明业务逻辑
- 路由键规则必须有中文文档
- 复杂消息流必须添加中文注释
- 禁止无意义注释

## 格式规范

- 配置代码缩进 4 空格
- 每个队列/交换机声明独占一块
- 路由键使用点分格式

## 性能优化

- 批量发送消息
- 使用 `channel.basicQos` 控制预取数量
- 使用仲裁队列替代镜像队列
- 合理设置消息 TTL
- 使用 Lazy Queue 处理堆积
- 连接池管理 Channel

## 集群规范

- 使用仲裁队列保证高可用，经典镜像队列（classic mirrored queue）已被官方废弃，存量需迁移
- 节点数至少 3 个（奇数）
- 使用 HAProxy / Nginx 负载均衡
- 监控队列长度和消费速率

## 安全规范

- TLS 加密：生产启用 AMQPS（5671）与管理界面 HTTPS（15671），明文端口（5672/15672）禁止对外网暴露
- 用户与权限：按 vhost 粒度授予用户 `configure` / `write` / `read` 三类权限（`rabbitmqctl set_permissions`），应用账号仅授予所属业务 vhost 的最小权限（vhost 隔离规范见上文）
- 禁用 guest 账号：删除默认 `guest` 账号（`rabbitmqctl delete_user guest`），禁止依赖"guest 仅限本机登录"的默认限制
- 管理界面访问控制：15672 仅对运维网段 / 堡垒机开放；管理员账号与业务账号分离，禁止应用使用管理员账号连接 Broker
- 敏感配置管理：连接密码通过环境变量或密钥管理服务注入，禁止写入代码与配置仓库；`rabbitmq.conf` 与证书文件权限收紧至仅运行用户可读
- 消息体保护：Broker 不提供端到端加密，消息体含敏感信息时必须在应用层加密后再入队

## 最佳实践

- 使用 Spring AMQP / RabbitMQ Client
- 使用死信队列 + 延迟队列实现定时任务
- 使用消息追踪（Firehose / Tracing）
- 使用 Federation / Shovel 跨数据中心
