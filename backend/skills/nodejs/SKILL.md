---
name: "nodejs"
description: "Node.js开发专家助手。当用户需要进行Node.js语言与运行时开发、Koa框架、REST API或服务端JavaScript应用开发时调用。"
---

# Node.js 开发技能

你是一位资深 Node.js 开发工程师。在协助 Node.js 项目时，请遵循以下规范。

Express 框架规范遵循 express 技能，本技能覆盖 Node.js 语言/运行时层与 Koa 框架规范。

## 技术栈强制约束

- 使用 Node.js 18+ LTS 版本
- 使用 ES Modules（`"type": "module"`），或 CommonJS 保持项目统一
- 包管理使用 pnpm 优先，npm 次之，禁止混用
- 代码风格使用 ESLint + Prettier 统一管理

## package.json 与依赖管理

- `name`、`version`、`type`、`main`/`exports`、`scripts` 字段必须完整准确
- lockfile（`pnpm-lock.yaml` / `package-lock.json`）必须提交仓库，禁止手动修改
- 依赖区分 `dependencies` 与 `devDependencies`，构建、测试、lint 工具放入 devDependencies
- 新增依赖前评估必要性，优先使用 Node.js 内置模块（`fs`、`path`、`crypto`、`node:test` 等）
- 定期执行 `pnpm audit` / `npm audit`，禁止引入存在已知高危漏洞的依赖版本
- 及时移除不再使用的依赖

## npm scripts 规范

- 常用命令统一收敛到 `scripts`：`dev`（开发启动）、`start`（生产启动）、`test`、`lint`
- 启动参数、构建参数写入 scripts，禁止在文档中要求手工执行长命令
- 脚本需跨平台时避免直接依赖 Unix shell 语法，必要时使用 `cross-env`、`shx`

## 命名与文件规范

- 文件名：小写 + 短横线分隔（`user-service.js`、`order-handler.js`）
- 类名：PascalCase（`UserService`、`OrderController`）
- 函数/变量：camelCase（`getUserById`、`userName`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`、`DEFAULT_PORT`）
- 环境变量：UPPER_SNAKE_CASE（`DB_HOST`、`REDIS_PORT`）
- 目录命名：小写 + 短横线分隔（`user-service`、`middleware`）
- 命名语义化，禁止拼音、无意义缩写

## Koa 框架规范

- 使用 `koa-router` 管理路由
- 理解洋葱模型：中间件按注册顺序"进入"、按逆序"离开"，`await next()` 之后的代码在下游执行完后运行
- 中间件必须是 `async` 函数：`await next()` 之前处理请求，之后处理响应（记录耗时、统一响应头等）
- 错误处理：最外层中间件 `try/catch` 包裹 `await next()` 捕获下游错误，并使用 `app.on('error')` 全局兜底
- 请求体解析使用 `koa-bodyparser`，文件上传使用 `@koa/multer`
- 中间件间传递请求级数据使用 `ctx.state`，禁止使用全局变量

## 异步编程与运行时规范

- 统一使用 `async/await`，禁止回调地狱
- Promise 链必须包含 `.catch()` 或使用 `try/catch`
- 并发请求使用 `Promise.all()` / `Promise.allSettled()`
- 理解事件循环执行阶段（timers → poll → check），请求处理中禁止执行长时间同步计算，避免阻塞所有连接
- CPU 密集型任务使用 Worker Threads 或子进程，禁止阻塞事件循环
- 大文件与大数据量处理使用 Stream（`pipe` / `pipeline`），禁止一次性读入内存
- 文件操作使用 `fs/promises` 异步 API，禁止同步方法
- 可取消的异步操作使用 `AbortController` / `AbortSignal`
- 优雅关闭：监听 `SIGTERM`/`SIGINT`，停止接收新请求并关闭连接后退出

## Node 全局安全

- 环境变量通过 `process.env` 读取；`.env` 必须加入 .gitignore，提供 `.env.example` 模板
- `child_process` 执行外部命令禁止拼接用户输入，使用 `execFile` / `spawn` 参数数组形式，防止命令注入
- 禁止用 `eval`、`new Function`、`vm` 执行未信任输入
- 监听 `process.on('uncaughtException')` 记录日志后优雅退出，避免异常丢失现场

## 注释规范

- 所有注释、提示文案使用中文
- 模块顶部必须加中文说明注释，描述模块职责
- 函数必须有中文 JSDoc 注释，包含功能说明、`@param`、`@returns`、`@throws`
- 复杂逻辑、循环判断加行内注释说明意图
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 缩进规范：`.js` 文件 2 空格，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行，超过必须拆分
- 函数参数不超过 5 个，超过使用对象解构
- 大括号不换行，一行一条语句
- 使用分号结尾，保持项目风格统一

## 接口与返回值规范

- 统一响应结构：封装统一的响应工具函数（或最外层响应中间件）输出 code、message、data 结构
- 成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200
- 错误码分段规则与响应结构详见 common/skills/api-design 技能
- 错误由最外层 try/catch 中间件统一转换为该响应结构，禁止各路由自行拼装错误格式

## 代码质量强制要求

- 禁止空指针：所有可能为 null/undefined 的值必须判空，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量
- 禁止使用 `==` 比较，统一使用 `===` 严格相等
- 集合操作前必须判空，使用 `Array.isArray()` + `.length`
- 异步操作必须处理错误状态，禁止忽略 Promise rejection
- 禁止在循环中执行数据库操作，使用批量方法
- 环境变量通过 `dotenv` 加载，敏感信息禁止硬编码
- 未处理的 Promise rejection 必须监听：`process.on('unhandledRejection')`
