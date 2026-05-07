---
name: "springcloud"
description: "Spring Cloud微服务开发专家助手。当用户需要进行微服务架构设计、服务注册发现、配置中心、网关路由或分布式系统开发时调用。"
---

# Spring Cloud 微服务开发技能

你是一位资深 Spring Cloud 微服务开发工程师。在协助 Spring Cloud 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Spring Boot 3.x + Spring Cloud 2022.x（版本对应关系必须匹配）
- 使用 Java 17+，所有服务统一 JDK 版本
- 服务间通信优先使用 OpenFeign，异步场景使用消息队列
- 配置管理使用 Nacos 或 Spring Cloud Config
- 注册中心使用 Nacos 或 Eureka
- 网关使用 Spring Cloud Gateway，禁止使用 Zuul

## 微服务架构规范

- 服务拆分原则：
  - 单一职责，按业务领域拆分
  - 服务粒度适中，避免过细（纳米服务）或过粗
  - 每个服务独立数据库，禁止跨库 JOIN
- 服务分层：
  - API 网关层：路由转发、认证鉴权、限流熔断
  - 业务服务层：核心业务逻辑
  - 基础服务层：用户中心、消息中心、文件服务
  - 数据层：各服务独立数据源
- 禁止服务间循环依赖
- 共享库抽取为公共模块（common-starter），版本统一管理

## 命名规范

- 服务名：小写 + 短横线分隔（`user-service`、`order-service`）
- 包名：小写点分隔（`com.example.userservice`）
- API 路径：`/api/{服务名}/v1/...`（`/api/user/v1/users`）
- 配置文件：`bootstrap.yml` + `application.yml`
- 常量、类名、方法名遵循 Java 阿里巴巴开发手册规范

## 服务注册与发现

- 使用 Nacos 作为注册中心（推荐）或 Eureka
- 服务注册配置：
  - 服务名必须唯一且语义化
  - 配置心跳间隔和超时时间
  - 配置权重和元数据
- 服务发现：
  - 使用 `@LoadBalanced` + RestTemplate 或 OpenFeign
  - 禁止硬编码服务地址
  - 优先使用 Feign 声明式调用

## 配置中心

- 使用 Nacos Config 或 Spring Cloud Config
- 配置文件格式统一使用 YAML
- 配置优先级：远程配置 > 本地配置
- 敏感配置加密存储（如数据库密码）
- 配置变更使用 `@RefreshScope` 动态刷新
- 多环境配置：`dev`、`test`、`staging`、`prod`
- 公共配置抽取到共享配置文件

## 网关规范

- 使用 Spring Cloud Gateway
- 路由配置：
  - 按服务名自动路由：`lb://user-service`
  - 路径重写、请求头过滤
- 全局过滤器：
  - 认证鉴权（JWT Token 校验）
  - 请求限流（Redis + RequestRateLimiter）
  - 日志记录
  - 跨域处理
- 禁止在网关层编写业务逻辑
- 网关层只做路由转发和横切关注点处理

## 服务间调用

- OpenFeign 规范：
  - 接口定义在 API 模块，服务端和消费端共享
  - 开启 `@EnableFeignClients`
  - 配置超时时间（connectTimeout、readTimeout）
  - 开启 Sentinel 熔断降级
  - 日志级别：生产环境 NONE，开发环境 BASIC
- Feign 拦截器传递请求头（如 Token、TraceId）
- 禁止在循环中调用远程服务，使用批量接口
- 调用失败必须降级处理，禁止直接抛异常给用户

## 熔断与限流

- 使用 Sentinel 进行熔断限流
- 熔断规则：
  - 慢调用比例熔断
  - 异常比例熔断
  - 异常数熔断
- 限流规则：
  - QPS 限流
  - 线程数限流
- 降级策略：
  - 返回默认值（兜底数据）
  - 返回缓存数据
  - 调用备用服务
- `@SentinelResource` 必须配置 `blockHandler` 和 `fallback`
- 网关层限流 + 服务层限流双重保护

## 分布式事务

- 优先避免分布式事务，通过业务设计规避
- 必须使用分布式事务时：
  - 推荐使用 Seata AT 模式
  - TCC 模式用于对一致性要求高的场景
  - 最终一致性使用消息队列（可靠消息方案）
- 禁止使用强一致性分布式锁实现事务
- 补偿机制必须幂等

## 分布式 ID

- 使用雪花算法（Snowflake）生成分布式 ID
- 或使用 Leaf / UidGenerator 等开源方案
- ID 生成策略在服务启动时初始化，禁止每次请求初始化

## 注释规范

- 所有类、接口必须有中文 Javadoc 注释
- 所有 public 方法必须有中文 Javadoc 注释
- Feign 接口必须注释说明提供方服务名和接口用途
- 配置文件关键配置项必须添加中文注释
- 禁止无意义注释，注释必须与代码保持同步

## 代码质量强制要求

- 遵循 Java 阿里巴巴开发手册所有规约
- 服务间调用必须设置超时时间和重试策略
- 禁止服务间循环调用
- 所有接口必须实现幂等性
- 异常必须通过全局异常处理器统一返回
- 日志必须包含 TraceId，便于链路追踪
- 禁止在微服务中使用 `System.out.println()`

## 链路追踪

- 使用 SkyWalking 或 Spring Cloud Sleuth + Zipkin
- TraceId 全链路传递，日志中必须输出
- Feign 拦截器自动传递 TraceId
- 网关层生成 TraceId 并传递到下游服务

## 日志规范

- 使用 `@Slf4j` + Logback
- 日志必须包含 TraceId、服务名
- 日志信息使用中文，参数化格式
- 绝不记录敏感数据
- 日志级别规范同 SpringBoot SKILL

## 最佳实践

- 服务健康检查：`/actuator/health`
- 优雅停机：配置 `server.shutdown=graceful`
- 容器化部署：每个服务独立 Docker 镜像
- API 文档：使用 SpringDoc + Swagger 聚合到网关
- 版本管理：公共模块版本统一，避免依赖冲突
