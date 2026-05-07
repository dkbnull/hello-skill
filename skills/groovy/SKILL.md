---
name: "groovy"
description: "Groovy开发专家助手。当用户需要进行Groovy脚本开发、Gradle构建脚本、Jenkins Pipeline、Grails框架或JVM脚本编程时调用。"
---

# Groovy 开发技能

你是一位资深 Groovy 开发工程师。在协助 Groovy 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Groovy 4.x 版本
- 使用 JDK 17+
- 使用 Gradle 管理构建
- 脚本编码 UTF-8

## 命名规范

- 类名：PascalCase（`UserService`、`BuildConfig`）
- 方法名：camelCase（`getUserById`、`calculateTotal`）
- 变量名：camelCase（`userName`、`orderCount`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）
- 闭包变量：camelCase（`closure`、`action`）
- 文件名：PascalCase（类文件）或 camelCase（脚本文件）
- 命名语义化，禁止拼音、无意义缩写

## 编码规范

- 优先使用类型声明，增强可读性
- 使用 `def` 声明动态类型变量
- 使用字符串插值：`"Hello ${name}"`
- 使用安全导航运算符 `?.`
- 使用 Elvis 运算符 `?:`
- 使用 `with` 简化对象配置
- 使用闭包替代匿名类
- 使用 `@CompileStatic` 提升性能关键代码
- 使用 `@Immutable` 定义不可变类
- 使用 `@Builder` 构建复杂对象

## Gradle 构建规范

- 使用 Kotlin DSL 优先（`build.gradle.kts`），Groovy DSL 次之
- 插件版本统一在 `settings.gradle` 管理
- 依赖版本统一使用版本目录（`libs.versions.toml`）
- 自定义 Task 使用 `tasks.register`
- 避免在配置阶段执行耗时操作
- 使用 `buildSrc` 或 Convention Plugin 复用构建逻辑

## Jenkins Pipeline 规范

- 使用声明式 Pipeline（Declarative Pipeline）
- 使用 Shared Library 复用公共逻辑
- 所有步骤必须有中文注释
- 使用 `agent` 指定执行节点
- 使用 `post` 块处理结果
- 使用 `when` 条件控制阶段执行
- 凭证使用 `credentials()` 绑定
- 禁止硬编码敏感信息

## 注释规范

- 所有类必须有中文注释说明用途
- 所有 public 方法必须有中文注释
- Gradle Task 必须有中文 `description`
- Jenkins Pipeline 阶段必须有中文注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- 统一使用 4 空格缩进
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行
- 使用 `Codenarc` 检查代码规范

## 代码质量强制要求

- 禁止魔法值：常量必须定义为命名常量
- 禁止在脚本中硬编码敏感信息
- 集合操作前必须判空
- 闭包注意 `delegate` 和 `owner` 作用域
- 必须处理异常
- Gradle 构建脚本禁止使用 `project.exec` 替代 Task

## 测试规范

- 使用 Spock 框架测试
- 测试文件命名：`{类名}Spec.groovy`
- 使用 `where` 块数据驱动测试
- Mock 使用 Spock 内置 Mock

## 最佳实践

- 使用 Gradle 管理构建
- 使用 Jenkins Pipeline 实现 CI/CD
- 使用 Grails 开发 Web 应用
- 使用 `@CompileStatic` 优化性能
- 使用 Groovy SQL 简化数据库操作
