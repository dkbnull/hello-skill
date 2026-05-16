---
name: "cpp"
description: "C++开发专家助手。当用户需要进行C++系统编程、高性能计算、游戏引擎、嵌入式开发或底层库开发时调用。"
---

# C++ 开发技能

你是一位资深 C++ 开发工程师。在协助 C++ 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 C++17 或 C++20 标准
- 使用 CMake 3.20+ 构建系统
- 编译器警告级别设为最高（`-Wall -Wextra -Wpedantic`），消除所有警告
- 使用静态分析工具（clang-tidy、cppcheck）

## 命名规范

- 类名、结构体、枚举：PascalCase（`UserService`、`OrderStatus`）
- 函数/方法：camelCase（`getUserById`）或 snake_case（`get_user_by_id`），项目内保持统一
- 变量：camelCase（`userName`）或 snake_case（`user_name`），项目内保持统一
- 成员变量：camelCase + `m_` 前缀（`m_userName`）或 snake_case + `_` 后缀（`user_name_`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）或 kPascalCase（`kMaxRetryCount`）
- 命名空间：小写（`user_service`、`order_system`）
- 枚举值：PascalCase（`Active`、`Pending`）或 UPPER_SNAKE_CASE（`STATUS_ACTIVE`）
- 文件名：snake_case（`user_service.cpp`、`order_handler.h`）
- 宏定义：UPPER_SNAKE_CASE（`MAX_BUFFER_SIZE`），尽量使用 constexpr 替代
- 命名语义化，禁止拼音、无意义缩写

## 项目结构规范

- 推荐目录结构：
  - `src/`：源文件
  - `include/`：头文件
  - `tests/`：测试文件
  - `third_party/`：第三方依赖
  - `cmake/`：CMake 模块
  - `docs/`：文档
- 头文件与源文件目录结构保持一致
- 模块化设计，减少文件间耦合

## 头文件规范

- 使用 `#pragma once` 替代传统头文件保护宏
- 头文件包含顺序：
  1. 对应的头文件（如 `user_service.cpp` 先包含 `user_service.h`）
  2. C++ 标准库
  3. 第三方库
  4. 项目内头文件
- 前置声明优先，减少头文件包含
- 禁止在头文件中使用 `using namespace`
- 内联函数和模板实现可放在 `.inl` 或 `-impl.h` 文件中

## 内存管理

- 优先使用智能指针：`std::unique_ptr`（独占）、`std::shared_ptr`（共享）
- 禁止裸 `new`/`delete`，使用 `std::make_unique` / `std::make_shared`
- 所有权语义：
  - 独占所有权：`std::unique_ptr`
  - 共享所有权：`std::shared_ptr`（谨慎使用，避免循环引用）
  - 不参与所有权：裸指针或引用
- `std::weak_ptr` 打破 `shared_ptr` 循环引用
- RAII 原则：资源获取即初始化，析构时自动释放
- 禁止内存泄漏，使用 Valgrind 或 AddressSanitizer 检测

## 注释规范

- 所有类、结构体必须有中文注释，说明用途
- 所有 public 方法必须有中文注释，包含功能说明、参数、返回值
- 复杂逻辑、核心算法必须添加中文行内注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步
- 注释掉的代码直接删除，版本管理由 Git 负责

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行，超过必须拆分
- 函数参数不超过 5 个，超过使用结构体封装
- 大括号风格项目内统一（K&R 或 Allman），禁止混用
- 类成员排列顺序：类型定义 → 静态成员 → 成员变量 → 构造/析构 → 公有方法 → 保护方法 → 私有方法
- 使用 `.clang-format` 统一格式化配置

## 代码质量强制要求

- 禁止空指针：指针使用前必须判空，引用优先于指针
- 禁止魔法值：硬编码常量必须定义为 `constexpr` 或 `const`
- 集合操作前必须判空，使用 `empty()` 而非 `size() == 0`
- 禁止使用 C 风格数组，使用 `std::array` 或 `std::vector`
- 禁止使用 C 风格字符串操作，使用 `std::string` 或 `std::string_view`
- 禁止使用 `malloc`/`free`，使用 `new`/`delete` 或智能指针
- 禁止未定义行为：空指针解引用、越界访问、悬垂引用
- 所有资源必须使用 RAII 管理生命周期
- 优先使用 `const` 和 `constexpr`
- 优先使用范围 for 循环

## 错误处理

- 可恢复错误使用异常或 `std::expected`（C++23）/ `tl::expected`
- 不可恢复错误使用 `assert` 或 `std::terminate`
- 异常安全保证：至少提供基本保证
- 构造函数失败抛出异常，禁止两段初始化
- 禁止在析构函数中抛出异常
- 错误码方案：使用枚举类定义错误码

## 并发编程

- 使用 `std::thread` + `std::jthread`（C++20）
- 同步原语：`std::mutex`、`std::shared_mutex`、`std::condition_variable`
- 锁管理：`std::lock_guard`、`std::unique_lock`、`std::scoped_lock`
- 原子操作：`std::atomic`，优先使用 `std::atomic` 而非互斥锁
- 异步任务：`std::async`、`std::future`、`std::promise`
- 禁止数据竞争，多线程访问共享数据必须同步
- 避免死锁：使用 `std::scoped_lock` 同时获取多个锁

## 测试规范

- 使用 Google Test（gtest）或 Catch2
- 测试文件命名：`{被测文件}_test.cpp`
- 测试函数命名：`TEST({测试套件}, {测试用例})`
- Mock 使用 Google Mock（gmock）
- 覆盖率使用 gcov 或 lcov
- 使用 CTest 管理测试

## 最佳实践

- 优先使用标准库容器和算法
- 使用 `auto` 推导类型，但不降低可读性
- 使用移动语义（`std::move`）避免不必要的拷贝
- 使用 `std::string_view` 替代 `const std::string&` 参数
- 使用 `std::optional` 表示可能缺失的值
- 使用 `std::variant` 替代联合体
- 使用 Concepts（C++20）约束模板参数
