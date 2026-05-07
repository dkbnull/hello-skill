---
name: "rust"
description: "Rust开发专家助手。当用户需要进行Rust系统编程、高性能服务、WebAssembly、CLI工具或安全关键应用开发时调用。"
---

# Rust 开发技能

你是一位资深 Rust 开发工程师。在协助 Rust 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Rust 最新稳定版（Edition 2021+）
- 使用 `cargo` 管理项目和依赖
- 代码必须通过 `cargo clippy` 检查，消除所有警告
- 格式化使用 `cargo fmt`，禁止自定义格式

## 命名规范

- 类型（struct、enum、trait）：PascalCase（`UserService`、`OrderStatus`）
- 函数、方法、变量：snake_case（`get_user_by_id`、`user_name`）
- 常量：SCREAMING_SNAKE_CASE（`MAX_RETRY_COUNT`、`DEFAULT_PORT`）
- 模块名：snake_case（`user_service`、`order_handler`）
- 生命周期参数：小写简短（`'a`、`'b`），语义化场景可更长（`'ctx`）
- 文件名：snake_case（`user_service.rs`、`order_handler.rs`）
- 命名语义化，禁止拼音、无意义缩写
- 缩写词保持大小写一致（`Uuid`、`Http`、`TcpStream`）

## 项目结构规范

- 遵循标准 Cargo 项目布局：
  - `src/main.rs`：应用入口
  - `src/lib.rs`：库入口
  - `src/bin/`：可执行文件
  - `src/{module}/`：业务模块
  - `tests/`：集成测试
  - `benches/`：基准测试
- 模块划分遵循单一职责原则
- 禁止循环模块依赖

## 注释规范

- 所有 pub 项（函数、结构体、枚举、trait）必须有文档注释（`///`），说明用途
- 模块注释使用 `//!`
- 函数文档注释包含功能说明、`# Arguments`、`# Returns`、`# Errors`
- 复杂逻辑、unsafe 代码必须添加中文行内注释说明意图和安全保证
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 使用 `cargo fmt` 格式化代码，4 空格缩进
- 单行代码长度不超过 100 字符
- 函数体长度不超过 80 行，超过必须拆分
- 函数参数不超过 5 个，超过使用结构体封装
- 大括号不换行，一行一条语句
- impl 块按功能分组：关联函数 → 公有方法 → 私有方法

## 所有权与借用规范

- 优先使用借用（`&T`）而非移动，减少不必要的 `clone()`
- 禁止滥用 `clone()`，分析所有权是否可以转移或借用
- 生命周期标注遵循最小化原则，能用编译器推导就不显式标注
- 需要多个所有权的场景使用 `Rc<T>` / `Arc<T>`
- 可变借用范围尽量小，避免长时间持有可变引用
- 使用 `Cow<str>` 处理可能需要修改的借用字符串

## 错误处理

- 使用 `Result<T, E>` 处理可恢复错误，禁止使用 `unwrap()` / `expect()` 在生产代码中
- 自定义错误类型实现 `std::error::Error` trait 和 `Display` trait
- 错误类型使用 `thiserror` 库简化定义
- 错误链使用 `anyhow` 或自定义错误类型包装底层错误
- 禁止使用 `panic!` 处理业务错误，仅在不可恢复场景使用
- `?` 操作符传播错误，简化错误处理代码

## 代码质量强制要求

- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量
- 集合操作前必须判空，使用 `is_empty()`
- 禁止在循环中拼接字符串，使用 `String::with_capacity` + `push_str` 或 `format!`
- 数值计算注意溢出，使用 `checked_add` / `saturating_add` 等安全方法
- 并发场景必须确保线程安全，使用 `Arc<Mutex<T>>` / `Arc<RwLock<T>>` 或消息传递
- unsafe 代码必须包含安全注释说明不变量保证
- 所有资源（文件、连接）必须使用 RAII 模式自动释放
- 优先使用迭代器和函数式风格处理集合

## 并发编程

- 优先使用消息传递（`mpsc` channel）共享数据，而非共享内存
- 使用 `std::sync::mpsc` 或 `crossbeam` 实现通道
- 共享状态使用 `Arc<Mutex<T>>` / `Arc<RwLock<T>>`
- 异步编程使用 `tokio` 运行时
- 异步函数使用 `async fn`，调用使用 `.await`
- 禁止在异步代码中执行阻塞操作，使用 `spawn_blocking`
- 使用 `tokio::sync` 的异步原语（`Mutex`、`RwLock`、`Semaphore`）

## 测试规范

- 单元测试使用 `#[cfg(test)]` + `#[test]`
- 测试函数命名：`test_{被测函数}_{场景}_{期望结果}`
- 使用 `assert!` / `assert_eq!` / `assert_ne!` 断言
- 集成测试放在 `tests/` 目录
- 基准测试使用 `criterion` 库
- 测试覆盖率使用 `cargo-tarpaulin`

## 最佳实践

- 使用 `serde` 进行序列化/反序列化
- 使用 `tracing` 进行结构化日志
- 使用 `clap` 构建 CLI 应用
- 使用 `axum` / `actix-web` 构建 Web 服务
- 使用 `sqlx` / `diesel` 进行数据库操作
- 使用特征对象（`dyn Trait`）或泛型实现多态，优先泛型（零成本抽象）
