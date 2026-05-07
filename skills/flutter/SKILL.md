---
name: "flutter"
description: "Dart + Flutter开发专家助手。当用户需要进行Flutter应用开发、组件开发、状态管理、导航或跨平台移动/桌面应用构建时调用。"
---

# Dart + Flutter 开发技能

你是一位资深 Dart + Flutter 开发工程师。在协助 Flutter 项目时，请遵循以下规范。

## 架构分层

- App 层：MaterialApp 组件、路由配置、应用主题
- Core 层：常量、扩展方法、HTTP 客户端、本地存储、工具函数
- Data 层：数据模型（JSON 序列化）、仓库实现、数据源（远程/本地）
- Domain 层：领域实体、仓库接口、用例（整洁架构）
- Presentation 层：页面组件、可复用组件、控制器、绑定
- 禁止跨层调用，Presentation 层不写业务逻辑

## 核心依赖

- 状态管理：GetX 或 Riverpod
- 网络：Dio
- 存储：shared_preferences、Hive
- 导航：GoRouter 或 GetX
- 模型：freezed_annotation、json_annotation、equatable
- UI：cached_network_image、shimmer、flutter_screenutil
- 工具：intl、logger、connectivity_plus

## 命名规范

- 类名：PascalCase（`UserController`、`OrderService`）
- 方法/变量：camelCase（`getUserById`、`userName`）
- 常量：camelCase（`maxRetryCount`）局部使用，PascalCase（`MaxRetryCount`）全局使用
- 文件：snake_case（`user_controller.dart`、`order_service.dart`）
- 目录：snake_case（`user_profile`、`order_list`）
- 私有成员：前缀下划线（`_internalCache`、`_handleRequest`）
- 命名语义化，禁止拼音、无意义缩写

## 注释规范

- 所有类、顶级函数必须有中文文档注释（`///`），说明用途和职责
- 所有 public 方法必须有中文文档注释，包含功能说明、参数、返回值
- 复杂业务逻辑、核心算法必须添加中文行内注释说明意图
- Widget 的 build 方法中复杂布局必须添加中文注释说明结构
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 2 空格缩进，禁止 Tab
- 单行代码长度不超过 80 字符
- 函数体长度不超过 50 行，超过必须拆分
- 函数参数不超过 5 个，超过使用数据类封装
- 使用 `dart format` 格式化代码，保持风格统一
- 类成员排列顺序：静态变量 → 实例变量 → 构造方法 → 公有方法 → 私有方法
- Widget 嵌套超过 3 层必须提取为独立 Widget 方法或组件

## 代码质量强制要求

- 禁止空指针：显式处理所有可能的空值情况，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量或枚举
  - 禁止：`if (status == 1)`
  - 正确：`if (status == UserStatus.active.code)`
- 集合操作前必须判空，使用 `isEmpty` / `isNotEmpty`
- 禁止在循环中拼接字符串，使用 `StringBuffer` 或 `join()`
- 所有资源（控制器、流、订阅）必须正确释放
- 使用 `dart analyze` 进行静态分析，消除所有警告
- UI 中必须处理所有加载、错误和空状态
- 尽可能使用 `const` 构造函数提升性能

## 主题配置

- 使用 `ThemeData` 并设置 `useMaterial3: true`
- 定义 `AppTheme` 类包含 lightTheme 和 darkTheme
- 将颜色、字符串和尺寸定义为常量（AppColors、AppStrings、AppDimens）

## 路由配置

- GetX：使用 `GetPage` 定义路由，配合 `Binding` 进行依赖注入
- GoRouter：使用 `GoRoute` 定义路由树
- 路由常量统一定义在 `AppRoutes` 类中

## 状态管理

- GetX：使用 `GetxController` 管理状态，`Obx` 实现响应式 UI 更新
- Riverpod：使用 `Provider` 和 `ConsumerWidget`
- 使用 GetX 控制器时优先使用 `GetView` 替代 `StatelessWidget`
- 通过 `Get.find()` 或依赖注入获取控制器

## API 客户端

- 使用 Dio 配合拦截器进行 API 调用
- 单例模式管理 Dio 实例
- 拦截器处理认证、日志等

## 仓库模式

- 使用抽象类定义仓库接口
- 具体实现分离数据访问逻辑
- 数据模型使用 `fromJson` 工厂构造函数进行 JSON 解析

## 测试规范

- 使用 `flutter_test` 进行单元测试
- 使用 `mockito` 进行模拟
- 使用 `@GenerateMocks` 注解生成 Mock 类
- 重点测试控制器和仓库逻辑

## 最佳实践

- 使用 `RefreshIndicator` 实现下拉刷新
- 复杂可滚动布局使用 `Slivers`
- 使用 `ScreenUtil` 实现响应式设计
- 使用 `Freezed` 创建不可变模型
- 使用 `json_serializable` 进行 JSON 解析
- 使用 `CachedNetworkImage` 进行图片缓存
