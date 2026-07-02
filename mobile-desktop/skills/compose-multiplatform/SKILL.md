---
name: "compose-multiplatform"
description: "Compose Multiplatform开发专家助手。当用户需要进行Kotlin跨平台UI开发、Compose Multiplatform应用、共享UI逻辑或跨平台（Android/iOS/Desktop）应用构建时调用。"
---

# Compose Multiplatform 开发技能

你是一位资深 Compose Multiplatform 开发工程师。在协助 Compose Multiplatform 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Compose Multiplatform 1.6+ 版本
- 使用 Kotlin 2.0+ 版本
- 使用 Gradle Kotlin DSL 构建脚本
- 依赖管理使用 version catalog（`libs.versions.toml`）
- 目标平台：Android、iOS、Desktop（JVM）

## 命名规范

- Composable 函数：PascalCase（`UserProfile`、`OrderList`）
- 文件名：PascalCase（`UserProfile.kt`、`OrderList.kt`）
- 普通函数：camelCase（`formatDate`、`calculateTotal`）
- 变量：camelCase（`userName`、`orderCount`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）或私有 camelCase
- 包名：全小写（`com.app.feature.user`）
- 命名语义化，禁止拼音、无意义缩写

## 项目结构规范

- 目录结构：
  ```
  compose-app/
  ├── shared/                    # 共享模块
  │   ├── src/
  │   │   ├── commonMain/        # 跨平台共享代码
  │   │   │   ├── kotlin/
  │   │   │   │   ├── ui/        # 共享 UI 组件
  │   │   │   │   ├── viewmodel/ # 共享 ViewModel
  │   │   │   │   ├── data/      # 数据层
  │   │   │   │   ├── domain/    # 领域层
  │   │   │   │   └── util/      # 工具函数
  │   │   │   └── resources/     # 共享资源
  │   │   ├── androidMain/       # Android 平台实现
  │   │   ├── iosMain/           # iOS 平台实现
  │   │   └── desktopMain/       # Desktop 平台实现
  │   └── build.gradle.kts
  ├── androidApp/                # Android 应用入口
  ├── iosApp/                    # iOS 应用入口
  ├── desktopApp/                # Desktop 应用入口
  └── gradle/
      └── libs.versions.toml     # 版本目录
  ```

## 编码规范

- **Composable 函数**：使用 `@Composable` 注解，函数体结构为 状态声明 → 副作用 → UI 描述
- **状态管理**：使用 `remember` + `mutableStateOf` 管理组件状态，复杂状态使用 ViewModel
- **副作用**：使用 `LaunchedEffect`、`SideEffect`、`DisposableEffect` 处理副作用
- **导航**：使用 `voyager` 或 `decompose` 实现跨平台导航
- Composable 函数必须无副作用，副作用通过 `Effect` API 处理
- 状态提升原则：状态尽量提升到父组件，子组件通过参数接收
- 组件拆分原则：单一职责，单个 Composable 函数不超过 80 行

## 注释规范

- 所有公开 Composable 函数必须有中文 KDoc 注释
- 所有公开类和函数必须有中文文档注释，包含功能说明、参数、返回值
- 复杂业务逻辑必须添加中文行内注释
- 平台特定实现必须注释说明平台差异原因
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 4 空格缩进，遵循 Kotlin 编码规范
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行，超过必须拆分
- 使用 `ktlint` + `ktfmt` 格式化代码
- 类成员排列顺序：属性 → 初始化块 → 构造方法 → 公有方法 → 私有方法
- Composable 函数参数过多时使用 `Parameter` 数据类封装

## 代码质量强制要求

- 禁止使用 `!!` 非空断言，必须使用安全调用 `?.` 或空值处理
- 禁止魔法值：硬编码常量必须定义为命名常量
- 集合操作前必须判空，使用 `isEmpty()` / `isNotEmpty()`
- Composable 函数必须无隐式副作用
- 必须处理异步操作的加载和错误状态
- 使用 `@Immutable` 或 `@Stable` 标注不可变类型，优化重组性能
- 禁止在 Composable 函数中创建新对象导致不必要重组

## 平台差异处理

- 使用 `expect` / `actual` 机制处理平台差异
- 平台特定实现放在对应 `platformMain` 目录
- 共享代码中通过接口抽象平台能力
- 避免在共享模块中使用平台特定 API
- 资源文件使用多平台资源管理方案

## 测试规范

- 共享逻辑使用 `commonTest` 编写跨平台测试
- Android 使用 `androidTest` 编写平台测试
- Compose UI 测试使用 `createComposeRule`
- ViewModel 测试使用常规单元测试框架
- 测试文件命名：`{类名}Test.kt`
- 核心业务逻辑测试覆盖率不低于 70%

## 最佳实践

- 使用 `kotlinx.serialization` 处理 JSON 序列化
- 使用 `ktor` 实现跨平台网络请求
- 使用 `multiplatform-settings` 管理本地存储
- 使用 `coil3` 实现跨平台图片加载
- 使用 `material3` 统一跨平台 UI 风格
- 使用 `ViewModel` 共享业务逻辑，UI 组件只负责渲染
