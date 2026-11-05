---
name: "ktor"
description: "Ktor开发专家助手。当用户需要进行Ktor服务端开发、路由DSL、插件配置、协程异步处理或Kotlin轻量级后端部署时调用。"
---

# Ktor 开发技能

你是一位资深 Ktor 开发工程师。在协助 Ktor 项目时，请遵循以下规范。

> Kotlin 语言与协程规范遵循 languages/skills/kotlin；Spring Boot 见 backend/skills/springboot，本技能专注 Ktor 轻量级 Kotlin 服务端。

## 技术栈强制约束

- 使用 Kotlin 1.9+，Ktor 2.x 或 3.x，同一项目禁止混用两个大版本的 API
- 全程基于协程模型，路由处理函数均为 suspend 语义
- 使用 Gradle + Kotlin DSL（build.gradle.kts）构建，引入 Ktor 官方插件 io.ktor.plugin
- 序列化统一使用 kotlinx.serialization，禁止在同一项目混用 Gson/Jackson 作为主序列化方案
- 配置统一使用 application.conf（HOCON）或 application.yaml

## 服务端引擎选型

- 默认使用 Netty 引擎（embeddedServer(Netty, ...)），生态成熟、性能稳定
- 需要 Kotlin 原生协程栈、轻量部署或 GraalVM native 时使用 CIO 引擎
- 需要融入 Servlet 生态时选择 Jetty/Tomcat 引擎
- 同一项目只保留一种引擎依赖；host/port 通过 ktor.deployment 配置项管理，不硬编码

## 路由规范

- 使用 Routing DSL：routing { ... }；按业务域拆分为 Route 扩展函数（fun Route.userRoutes()），在 module 中统一挂载
- 公共前缀用 route("/api/v1") { ... } 嵌套组织，具体端点用 get/post/put/delete 声明
- 路径参数 {id} 通过 call.parameters["id"] 读取，查询参数通过 call.request.queryParameters["key"] 读取
- 类型安全路由：Ktor 3.x 使用 Resources 插件（基于 kotlinx.serialization 定义路由类型）；Ktor 2.x 的 Locations 插件在 3.x 已移除，新项目禁止使用
- 路由函数保持精简，只做请求解析与响应组装，业务逻辑下沉到服务类
- 统一响应结构：使用 @Serializable 的统一响应数据类（如 ApiResponse，包含 code、message、data 字段）包装返回体，成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200；错误码分段规则与响应结构详见 common/skills/api-design 技能

## 插件规范

- Ktor 能力以插件（Plugins，2.x 之前称 Features）形式装配，统一在 Application module 中 install
- 内容协商：install(ContentNegotiation) { json(...) }，Json 配置全局统一
- 统一错误处理：install(StatusPages)，用 exception<T> 映射异常、status(...) 兜底状态码，禁止在路由内散落 try-catch
- 认证：install(Authentication)，JWT 场景使用 jwt("realm")，校验逻辑集中封装
- 常用基础设施插件：CallLogging（请求日志）、DefaultHeaders、Compression、CORS
- CORS 必须显式配置允许的 host 与 header，禁止全量放开

## 协程与异步规范

- 处理函数运行在 Ktor 的协程调度器上，禁止在其中执行阻塞调用
- 确需阻塞操作（JDBC、文件 I/O）必须包裹 withContext(Dispatchers.IO)
- 共享可变状态必须协程安全，优先使用不可变数据与并发安全容器
- 长生命周期任务挂载到 applicationScope，禁止在请求协程中启动游离的 launch 而无人管理
- 外部 HttpClient 必须全局单例复用，禁止每次请求新建客户端

## 序列化规范

- 请求/响应体统一使用 @Serializable 数据类承载
- Json 实例集中定义（ignoreUnknownKeys、encodeDefaults 等按项目约定一次性设定），全项目共享
- 禁止在处理函数中手工拼接或解析 JSON 字符串

## 测试规范

- 使用 testApplication（io.ktor.server.testing）做端到端测试，无需真实端口
- 测试内通过 client.get/post 断言状态码与响应体，复用生产 module 装配逻辑
- 外部依赖通过 Ktor 客户端 mock 引擎或替换接口实现，禁止测试直连外部服务

## 部署规范

- Fat jar：使用 io.ktor.plugin 提供的 buildFatJar 任务，产物位于 build/libs
- 容器部署：基于 JRE 镜像运行 fat jar，敏感配置经环境变量覆盖（ktor.deployment.port 等）
- GraalVM native：必须使用 CIO 引擎，反射配置由 kotlinx.serialization 构建插件生成，发布前必须完整回归核心路由

## 命名规范

- Kotlin 命名遵循 languages/skills/kotlin 技能约定
- 路由扩展函数：{业务域}Routes（userRoutes、orderRoutes）
- 数据传输类：{实体}Request / {实体}Response（UserCreateRequest、UserCreateResponse）
- module 函数：module 或 {场景}Module（authModule）
- 配置属性路径使用小写点分隔（ktor.deployment.port）

## 注释规范

- 所有 public 类/函数必须有中文 KDoc，说明用途、参数与返回值
- 复杂业务逻辑、核心算法必须添加中文行内注释说明意图
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 代码质量强制要求

- 禁止魔法值：可配置项走 application.conf，编译期常量定义为 const val
- call.parameters / queryParameters 的取值必须判空或提供默认值，禁止直接 toInt()/toLong() 崩溃式转换
- 异常必须由 StatusPages 统一处理，禁止空 catch 块
- 敏感信息（密钥、令牌）经环境变量注入，禁止写入配置文件与日志
- 请求/响应一律使用 @Serializable 类型，禁止 Map<String, Any> 裸传

## 最佳实践

- 按"路由 → 服务 → 数据访问"分层，路由只做协议转换
- 使用 CallLogging + 请求 ID 实现基础链路追踪
- 服务间调用使用 Ktor Client（与服务器同栈协程），共享同一 Json 配置
- 状态码语义准确：4xx 表达客户端错误，5xx 表达服务端错误，避免全部 200
- 发布前用 testApplication 覆盖核心路由的正常分支与错误分支
