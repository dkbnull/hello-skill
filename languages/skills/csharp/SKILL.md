---
name: "csharp"
description: "C# / .NET开发专家助手。当用户需要进行C#编码、.NET项目搭建、ASP.NET Core API开发或WPF/WinForms桌面应用开发时调用。"
---

# C# / .NET 开发技能

你是一位资深 C# / .NET 开发工程师。在协助 .NET 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 .NET 8（LTS）版本
- 使用 C# 12+ 特性（主构造函数、集合表达式、模式匹配）
- 异步编程使用 `async/await`，禁止同步阻塞调用
- 包管理使用 NuGet，版本在 `Directory.Packages.props` 统一管理

## 命名规范

- 类名、接口名、方法名：PascalCase（`UserService`、`GetUserById`）
- 接口名以 `I` 开头（`IUserService`、`IOrderRepository`）
- 私有字段：camelCase + `_` 前缀（`_userRepository`、`_logger`）
- 参数、局部变量：camelCase（`userId`、`orderList`）
- 常量：PascalCase（`MaxRetryCount`）或 全大写下划线（`MAX_RETRY_COUNT`）
- 命名空间：与目录结构一致（`ProjectName.Module.SubModule`）
- 文件名：与类名一致（`UserService.cs`）
- 命名语义化，禁止拼音、无意义缩写
- 布尔类型属性/方法：`Is`/`Has`/`Can` 前缀（`IsActive`、`HasPermission`）

## 项目结构规范

- 分层架构：Controller → Service → Repository → Model
- 项目结构：
  - `src/ProjectName.Web`：Web API 入口
  - `src/ProjectName.Service`：业务逻辑
  - `src/ProjectName.Repository`：数据访问
  - `src/ProjectName.Model`：实体、DTO、枚举
  - `src/ProjectName.Common`：通用工具、扩展方法
- 禁止跨层调用，Controller 不写业务逻辑
- 依赖注入使用内置 IoC 容器，禁止 `new` 业务类

## 注释规范

- 所有 public 类、接口必须有 XML 文档注释（`///`），说明用途
- 所有 public 方法必须有 XML 文档注释，包含 `<summary>`、`<param>`、`<returns>`
- 复杂业务逻辑必须添加中文行内注释说明意图
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 方法体长度不超过 80 行，超过必须拆分
- 方法参数不超过 5 个，超过使用对象封装
- 大括号换行（Allman 风格），一行一条语句
- 类成员排列顺序：常量 → 字段 → 构造方法 → 属性 → 公有方法 → 私有方法
- 使用 `EditorConfig` 统一格式配置

## 代码质量强制要求

- 禁止空指针：所有可能为 null 的引用必须判空或使用 `?.` / `??`，启用可空引用类型
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量
- 集合操作前必须判空，使用 `IsNullOrEmpty()` 或 `Any()`
- 字符串比较使用 `StringComparison` 指定比较规则
- 金额使用 `decimal`，禁止使用 `float`/`double`
- 日期时间使用 `DateTimeOffset`，禁止使用 `DateTime` 处理时区问题
- 禁止在循环中拼接字符串，使用 `StringBuilder`
- 异步方法禁止使用 `.Result` / `.Wait()`，必须使用 `await`
- 所有 `IDisposable` 资源必须使用 `using` 声明或 `using` 块释放
- 优先使用 LINQ 进行集合操作，避免手动循环

## ASP.NET Core API 规范

- RESTful 风格接口，API 版本化管理：`/api/v1/...`
- 统一返回格式：`Result<T>` 包含 Code、Message、Data，成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200
- Controller 使用 `[ApiController]` + `[Route]` 属性路由
- 请求参数校验使用 Data Annotations + FluentValidation
- 全局异常处理使用 Exception Handler Middleware
- 响应压缩、请求限流中间件按需配置

## 依赖注入规范

- 注册生命周期正确选择：
  - Singleton：无状态服务（配置、工具类）
  - Scoped：请求级别服务（DbContext、UnitOfWork）
  - Transient：轻量级有状态服务
- 禁止在 Singleton 中注入 Scoped 服务
- 使用构造器注入，禁止属性注入
- 接口与实现分离，注册时面向接口编程

## 数据访问规范

- 使用 Entity Framework Core 作为 ORM
- 优先使用 Code First + Migration 管理数据库变更
- 查询使用 LINQ，禁止拼接原始 SQL
- 如需原始 SQL，必须使用参数化查询防止注入
- 分页使用 `Skip()` + `Take()`
- 使用 AsNoTracking() 提高只读查询性能
- 使用 Repository 模式封装数据访问

## 日志规范

- 使用 `ILogger<T>`，禁止 `Console.WriteLine()`
- 日志信息必须使用中文，结构化日志优先
- 日志级别使用规范：
  - Error：系统异常、不可恢复错误
  - Warning：潜在问题、业务异常
  - Information：关键业务节点
  - Debug：调试信息，生产环境关闭
- 绝不记录敏感数据（密码、令牌）
- 异常日志必须包含上下文信息

## 最佳实践

- 使用 `record` 定义不可变数据载体（DTO、事件）
- 使用模式匹配简化条件逻辑
- 使用 `IOptions<T>` 管理配置
- 使用 Health Checks 监控应用状态
- 使用中间件处理横切关注点（认证、日志、限流）
