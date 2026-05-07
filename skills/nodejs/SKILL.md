---
name: "nodejs"
description: "Node.js开发专家助手。当用户需要进行Node.js后端开发、Express/Koa框架、REST API或服务端JavaScript应用开发时调用。"
---

# Node.js 开发技能

你是一位资深 Node.js 开发工程师。在协助 Node.js 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Node.js 18+ LTS 版本
- 使用 ES Modules（`"type": "module"`），或 CommonJS 保持项目统一
- 包管理使用 pnpm 优先，npm 次之，禁止混用
- 代码风格使用 ESLint + Prettier 统一管理

## 命名与文件规范

- 文件名：小写 + 短横线分隔（`user-service.js`、`order-handler.js`）
- 类名：PascalCase（`UserService`、`OrderController`）
- 函数/变量：camelCase（`getUserById`、`userName`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`、`DEFAULT_PORT`）
- 环境变量：UPPER_SNAKE_CASE（`DB_HOST`、`REDIS_PORT`）
- 目录命名：小写 + 短横线分隔（`user-service`、`middleware`）
- 命名语义化，禁止拼音、无意义缩写

## 项目结构规范

- 推荐目录结构：
  - `src/controllers/`：请求处理
  - `src/services/`：业务逻辑
  - `src/models/`：数据模型
  - `src/routes/`：路由定义
  - `src/middleware/`：中间件
  - `src/utils/`：工具函数
  - `src/config/`：配置管理
- 分层原则：Controller → Service → Model，禁止跨层调用
- 路由定义与业务逻辑分离，Controller 不写业务逻辑

## 框架规范

### Express
- 使用 `express.Router()` 模块化路由
- 中间件顺序：日志 → 认证 → 路由 → 错误处理
- 错误处理中间件必须放在最后：`app.use(errorHandler)`

### Koa
- 使用 `koa-router` 管理路由
- 中间件使用 `async/await`
- 使用 `app.on('error')` 全局错误处理

## 异步编程规范

- 统一使用 `async/await`，禁止回调地狱
- Promise 链必须包含 `.catch()` 或使用 `try/catch`
- 并发请求使用 `Promise.all()` / `Promise.allSettled()`
- 长时间运行任务使用 Worker Threads，禁止阻塞事件循环
- 文件操作使用 `fs/promises` 异步 API，禁止同步方法

## 注释规范

- 所有注释、提示文案使用中文
- 模块顶部必须加中文说明注释，描述模块职责
- 函数必须有中文 JSDoc 注释，包含功能说明、`@param`、`@returns`、`@throws`
- 复杂逻辑、循环判断加行内注释说明意图
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 缩进规范：`.js` 文件 4 空格，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行，超过必须拆分
- 函数参数不超过 5 个，超过使用对象解构
- 大括号不换行，一行一条语句
- 使用分号结尾，保持项目风格统一

## 代码质量强制要求

- 禁止空指针：所有可能为 null/undefined 的值必须判空，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量
- 禁止使用 `==` 比较，统一使用 `===` 严格相等
- 集合操作前必须判空，使用 `Array.isArray()` + `.length`
- 异步操作必须处理错误状态，禁止忽略 Promise rejection
- 禁止在循环中执行数据库操作，使用批量方法
- 环境变量通过 `dotenv` 加载，敏感信息禁止硬编码
- 未处理的 Promise rejection 必须监听：`process.on('unhandledRejection')`

## API 规范

- RESTful 风格接口，API 版本化管理：`/api/v1/...`
- 统一返回格式：`{ "code": 0, "msg": "成功", "data": {} }`
- 请求参数校验使用 `joi` 或 `zod`
- 文件上传限制大小和类型
- 接口必须添加限流中间件防止滥用

## 日志规范

- 使用 `winston` 或 `pino`，禁止 `console.log()`
- 日志信息必须使用中文
- 日志级别使用规范：
  - ERROR：系统异常、不可恢复错误
  - WARN：潜在问题、业务异常
  - INFO：关键业务节点
  - DEBUG：调试信息，生产环境关闭
- 绝不记录敏感数据（密码、令牌）
- 异常日志必须包含请求上下文

## 安全规范

- 使用 `helmet` 设置安全响应头
- 使用 `cors` 配置跨域白名单，禁止 `*`
- 使用 `express-rate-limit` 限流
- 密码使用 `bcrypt` 加密存储
- JWT Token 设置合理过期时间
- SQL 参数化查询，防止注入
- 输入校验和输出编码，防止 XSS

## 最佳实践

- 使用 `dotenv` 管理环境变量
- 使用连接池管理数据库连接
- 优雅关闭：监听 `SIGTERM`/`SIGINT`，关闭连接后退出
- 健康检查接口：`/health`
- 使用 `pm2` 或 `docker` 部署，开启集群模式
