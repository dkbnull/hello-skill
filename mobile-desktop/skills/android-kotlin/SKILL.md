---
name: "android-kotlin"
description: "Android Kotlin开发专家助手。当用户需要进行Android Kotlin应用开发、Jetpack Compose、协程Flow、MVVM架构或现代Android开发时调用。"
---

# Android Kotlin 开发技能

你是一位资深 Android Kotlin 开发工程师。在协助 Android Kotlin 项目时，请遵循以下规范。

Kotlin 语言层规范遵循 languages 下的 kotlin 技能，本技能仅补充 Android 平台规范。

## 技术栈强制约束

- 使用 Kotlin 作为主要开发语言
- 最低 SDK 版本：API 24（Android 7.0），目标 SDK 版本：API 34+
- 使用 Jetpack 组件（ViewModel、Flow、Navigation、Room）
- 使用 Gradle Kotlin DSL 构建脚本
- UI 开发优先使用 Jetpack Compose

## 命名规范

- 类名：PascalCase（`UserViewModel`、`OrderRepository`）
- 函数/变量：camelCase（`getUserById`、`userName`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）或 PascalCase（顶层常量）
- 包名：小写点分隔（`com.example.app.feature.user`）
- 布局文件：snake_case（`activity_main.xml`、`fragment_user_list.xml`、`item_order.xml`）
- 资源文件：
  - drawable：`ic_{名称}`（图标）、`bg_{名称}`（背景）
  - values：`colors.xml`、`strings.xml`、`dimens.xml`、`styles.xml`
- 命名语义化，禁止拼音、无意义缩写

## Kotlin 惯用语法

- 优先使用 `val`，仅在需要重新赋值时使用 `var`
- 使用数据类（`data class`）承载数据
- 使用密封类（`sealed class`）表示有限状态集
- 使用扩展函数增强已有类功能
- 使用作用域函数：`let`、`run`、`with`、`apply`、`also`
- 使用 `when` 表达式替代多重 `if-else`
- 使用字符串模板（`"Hello, $name"`）替代字符串拼接
- 使用解构声明（`val (name, age) = person`）

## 架构规范

- 推荐架构：MVVM + Clean Architecture
- 分层结构：
  - `ui/`：Activity、Fragment、Compose Screen、ViewModel
  - `domain/`：UseCase、Repository 接口、Entity
  - `data/`：Repository 实现、Remote DataSource、Local DataSource、DTO
  - `di/`：依赖注入模块
- 禁止跨层调用，UI 层不直接访问 Data 层
- 依赖注入使用 Hilt

## Jetpack 组件规范

- **ViewModel**：持有 UI 状态，禁止持有 View 引用
- **StateFlow / SharedFlow**：替代 LiveData 进行状态管理
- **Navigation**：页面导航使用 Navigation Component，禁止手动管理 Fragment 事务
- **Room**：本地数据库使用 Room，DAO 方法返回 Flow 实现数据观察
- **DataStore**：替代 SharedPreferences 存储键值对
- **WorkManager**：后台任务使用 WorkManager，禁止直接使用 Service
- **Paging 3**：分页加载使用 Paging 库

## Jetpack Compose 规范

- 组件函数使用 `@Composable` 注解
- 状态提升原则：无状态组件 + 状态参数
- 组件拆分：单个 Composable 函数不超过 100 行
- 预览使用 `@Preview` 注解
- 主题系统：Material Theme 自定义颜色、字体、形状
- 列表使用 `LazyColumn` / `LazyRow`，禁止 `Column` + `forEach`

## 协程与 Flow 规范

- 结构化并发：使用 `CoroutineScope` 管理协程生命周期
- 调度器选择：
  - `Dispatchers.Main`：UI 操作
  - `Dispatchers.IO`：网络/文件 IO
  - `Dispatchers.Default`：CPU 密集型
- 异步操作使用 `suspend` 函数
- 并发启动使用 `launch`（不返回结果）或 `async`（返回结果）
- Flow 操作符链式调用，避免在 Flow 中使用可变状态
- 共享 Flow 使用 `shareIn` / `stateIn`
- 协程必须处理异常和取消

## 网络请求规范

- 使用 Retrofit + OkHttp
- OkHttp 拦截器：
  - 请求拦截器：添加 Token、TraceId
  - 响应拦截器：统一错误处理
- 协程 + Flow 处理异步请求
- 返回结果封装：`Result<T>` 或 `ApiResult<T>`，包含 code、message、data，成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200；错误码分段规则与响应结构详见 common/skills/api-design 技能
- JSON 解析使用 kotlinx.serialization 或 Moshi
- 网络状态监听使用 `ConnectivityManager`

## 空安全规范

- 禁止使用 `!!` 强制非空断言（测试代码除外）
- 使用 `?.` 安全调用 + `?:` 空值合并
- 使用 `let` 处理可空值：`nullable?.let { ... }`
- 使用 `require` / `check` 进行前置条件校验
- 平台类型（Java 互操作）必须显式标注可空性

## 注释规范

- 所有类、接口必须有中文 KDoc 注释（`/** */`）
- 所有 public 方法必须有中文 KDoc 注释
- 复杂业务逻辑必须添加中文行内注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行，超过必须拆分
- 函数参数不超过 5 个，超过使用 data class 封装
- 大括号不换行（K&R 风格）
- 类成员排列顺序：伴生对象 → 属性 → 初始化块 → 构造方法 → 公有方法 → 私有方法
- trailing comma：多行声明使用尾逗号

## 代码质量强制要求

- 禁止空指针：使用 Kotlin 空安全特性，禁止 `!!`
- 禁止魔法值：硬编码常量必须定义为命名常量
- 集合操作前必须判空，使用 `isEmpty()` / `isNotEmpty()`
- 禁止在主线程执行 IO 操作，使用协程 `Dispatchers.IO`
- 协程必须处理异常和取消
- 禁止内存泄漏：Activity/Fragment 销毁时取消协程和回调
- 字符串资源统一放入 `strings.xml`，禁止硬编码
- 优先使用不可变集合（`listOf`、`mapOf`），仅在需要修改时使用可变集合
- 使用 `ktlint` + `detekt` 进行静态检查

## 性能优化

- 布局优化：减少嵌套层级，使用 ConstraintLayout
- 图片加载使用 Coil 或 Glide，禁止手动解码
- 列表使用 DiffUtil 或 Paging 优化刷新
- 避免在 `onDraw` 中创建对象
- 使用 Baseline Profile 优化启动
- Compose 性能：使用 `remember`、`derivedStateOf`、`key` 避免不必要的重组

## 测试规范

- 单元测试使用 JUnit 5 + MockK（Kotlin 原生 Mock 框架）
- UI 测试使用 Compose Testing
- 测试命名：`should_{期望行为}_when_{条件}`，该 BDD 风格为框架惯例豁免，默认命名见 common 下的 testing 技能，同一项目内风格必须统一。
- ViewModel 测试使用 Turbine 验证 Flow
- Repository 测试使用 MockWebServer
- 协程测试使用 `runTest`

## 签名与发布

- 使用 `keytool` 生成发布 keystore，密码通过 `keystore.properties`（加入 .gitignore）或环境变量注入 `build.gradle.kts` 的 `signingConfigs`，禁止硬编码
- keystore 务必在异地保留副本，一旦丢失将永久失去为已发布应用推送更新的能力
- 发布使用 Android App Bundle：`./gradlew bundleRelease`，Google Play 强制 AAB 格式，建议开启 Play App Signing
- release 构建开启 R8 混淆（`isMinifyEnabled = true`）与资源收缩（`isShrinkResources = true`），维护 keep 规则
- 版本号：`versionCode` 每次发版必须递增，`versionName` 采用语义化版本命名
- 应用内更新：受国内市场合规约束，更新需跳转至应用当前所在官方商店完成，不得引导用户自行下载 APK 安装；海外接入 Google Play In-App Updates API

## 推送通知

- 海外使用 FCM（firebase-messaging），国内接入华为 HMS Push、小米、OPPO、vivo、荣耀等厂商通道，推荐极光、个推等聚合 SDK 统一接入
- Android 8.0+ 必须创建 `NotificationChannel`，按业务划分渠道
- Android 13+ 需动态申请 `POST_NOTIFICATIONS` 权限
- Compose 中使用 `rememberLauncherForActivityResult(ActivityResultContracts.RequestPermission())` 申请通知权限
- 通知的 PendingIntent 需声明 `FLAG_IMMUTABLE`，Android 12 起系统强制校验
- 点击跳转通过深链或 Intent extras 处理，数据消息依赖厂商通道长连接到达

## 运行时权限

- 定位、相机、麦克风、存储等危险权限须在运行时向用户发起授权，仅在清单中声明并不会自动获得
- Compose：`rememberLauncherForActivityResult(ActivityResultContracts.RequestPermission()) { granted -> ... }`，在 `LaunchedEffect` 中触发申请
- 非 Compose：`registerForActivityResult(ActivityResultContracts.RequestPermission())` 替代传统 `onRequestPermissionsResult` 回调
- 申请前检查：`ContextCompat.checkSelfPermission()`；多权限使用 `RequestMultiplePermissions`
- 处理拒绝：`shouldShowRequestPermissionRationale()` 为 true 先弹用途说明，永久拒绝引导用户到系统设置页
- 敏感权限按需索取：待相关功能首次触发时再发起申请，不允许应用启动时集中索要全部权限

## 最佳实践

- 使用协程 + Flow 替代 RxJava 和回调
- 使用 Hilt 管理依赖注入
- 使用 Navigation Component 管理页面导航
- 使用 Jetpack Compose 构建 UI
- 使用 LeakCanary 检测内存泄漏
- 使用 R8 混淆和优化
- 使用 `value class` 包装基础类型增强类型安全
