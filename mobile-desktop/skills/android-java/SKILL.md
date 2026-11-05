---
name: "android-java"
description: "Android Java开发专家助手。当用户需要进行Android Java应用开发、传统View体系、Jetpack组件、MVVM架构或遗留Android项目维护时调用。"
---

# Android Java 开发技能

你是一位资深 Android Java 开发工程师。在协助 Android Java 项目时，请遵循以下规范。

通用 Android 规范（命名、架构分层、Jetpack 通用组件、网络、性能）遵循 android-kotlin 技能，本技能仅覆盖 Java 语言与 View 体系/XML/RxJava 特有约定。

## 技术栈强制约束

- 使用 Java 8+ 作为开发语言
- 使用 Jetpack 组件（ViewModel、LiveData、Navigation、Room）
- 使用 Gradle Groovy DSL 或 Kotlin DSL 构建脚本
- UI 开发使用传统 View 体系 + XML 布局

## 命名规范

- 遵循 Java 阿里巴巴开发手册命名规约

## 架构规范

- 依赖注入使用 Hilt 或 Dagger2

## Jetpack 组件规范

- **LiveData**：UI 层状态观察，生命周期感知
- **Room**：本地数据库使用 Room，DAO 方法返回 LiveData 或 RxJava
- **ViewBinding**：替代 `findViewById`，禁止使用 ButterKnife

## View 体系规范

- 布局优化：
  - 减少嵌套层级，使用 ConstraintLayout
  - 使用 `<include>` 和 `<ViewStub>` 复用布局
  - 使用 Merge 标签减少根层级
- Adapter 规范：
  - 使用 `RecyclerView.Adapter` + `ViewHolder`
  - 必须实现 DiffUtil 优化刷新
  - 禁止在 `onBindViewHolder` 中创建对象
- 自定义 View：
  - 继承适当父类（View、ViewGroup、TextView）
  - 实现构造函数链
  - 支持自定义属性（`attrs.xml`）
  - 重写 `onMeasure` / `onLayout` / `onDraw`

## 网络请求规范

- 异步请求使用 RxJava 或 Callback
- JSON 解析使用 Gson 或 Moshi

## 注释规范

- 所有类、接口必须有中文 Javadoc 注释（`/** */`）
- 所有 public 方法必须有中文 Javadoc 注释
- 复杂业务逻辑必须添加中文行内注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 类成员排列顺序：静态变量 → 成员变量 → 构造方法 → 公有方法 → 私有方法
- 遵循 Java 阿里巴巴开发手册格式规约

## 代码质量强制要求

- 禁止空指针：所有对象使用前必须判空，使用 `Optional` 或空判断
- 集合操作前必须判空，使用 `isEmpty()` / `CollectionUtils.isEmpty()`
- 禁止在主线程执行 IO 操作，使用 RxJava 或 AsyncTask（已废弃，推荐 RxJava）
- 禁止内存泄漏：
  - Activity/Fragment 销毁时取消 RxJava 订阅（`CompositeDisposable`）
  - 静态集合持有 Activity 引用必须及时清理
  - Handler 使用弱引用
  - 匿名内部类持有外部引用注意生命周期
- 遵循 Java 阿里巴巴开发手册所有规约
- 使用 `Checkstyle` + `PMD` + `SpotBugs` 进行静态检查

## 异步编程规范

- 使用 RxJava 处理异步操作
- 线程调度：
  - `Schedulers.io()`：网络/文件 IO
  - `AndroidSchedulers.mainThread()`：UI 操作
  - `Schedulers.computation()`：CPU 密集型
- CompositeDisposable 管理订阅，`onDestroy` 时 `clear()`
- 禁止回调地狱，使用 RxJava 链式操作
- 异常处理使用 `onError` 回调，禁止忽略

## 性能优化

- 图片加载使用 Glide，禁止手动解码
- 使用 `StrictMode` 检测主线程 IO 和网络操作
- 内存优化：避免大对象频繁创建，使用对象池

## 测试规范

- 单元测试使用 JUnit 4/5 + Mockito
- UI 测试使用 Espresso
- 测试命名：`test_{方法名}_{场景}_{期望结果}`
- ViewModel 测试使用 InstantTaskExecutorRule

## 签名与发布

- 使用 `keytool` 生成发布 keystore：`keytool -genkeypair -v -keystore release.keystore -alias app -keyalg RSA -keysize 2048 -validity 10000`
- keystore 与密码严禁提交仓库，通过 `local.properties`（加入 .gitignore）或环境变量注入，并异地备份（签名丢失将无法再发布更新）
- `build.gradle` 配置 `signingConfigs.release` 引用上述变量，release 构建开启 `minifyEnabled` + R8 混淆与资源收缩
- 发布使用 Android App Bundle（AAB）：`./gradlew bundleRelease`，Google Play 强制 AAB 格式；国内部分市场仅支持 APK，用 `./gradlew assembleRelease` 补充构建
- 版本管理：`versionCode` 单调递增，`versionName` 遵循语义化版本
- 应用内更新合规：国内上架必须跳转本应用所在官方市场更新，禁止应用内引导自下载 APK；海外使用 Google Play In-App Updates API

## 推送通知

- 海外使用 FCM（Firebase Cloud Messaging）；国内设备无法稳定连接 FCM，需接入华为 HMS Push、小米、OPPO、vivo、荣耀等厂商通道，推荐使用极光、个推等聚合 SDK 统一接入（厂商通道为主，自建长连接兜底）
- Android 8.0+ 必须创建 `NotificationChannel`，按业务划分渠道，支持用户在系统设置中关闭
- Android 13+ 通知需动态申请 `POST_NOTIFICATIONS` 权限，被拒绝后不展示通知
- 通知 PendingIntent 必须设置 `FLAG_IMMUTABLE`（Android 12+ 强制要求）

## 运行时权限

- 危险权限（定位、相机、麦克风、存储等）必须在运行时动态申请，清单声明只是前提
- 申请前检查：`ContextCompat.checkSelfPermission()`，已授权直接放行
- 推荐使用 ActivityResult API：`registerForActivityResult(ActivityResultContracts.RequestPermission())`，兼容场景可用 `ActivityCompat.requestPermissions()`
- 处理拒绝：`shouldShowRequestPermissionRationale()` 返回 true 时先弹用途说明，再次拒绝引导用户到系统设置页
- 多权限申请使用 `ActivityResultContracts.RequestMultiplePermissions()`，按结果分别处理
- 敏感权限遵循最小化原则，按功能触发时申请，禁止启动时全量申请

## 最佳实践

- 使用 RxJava 替代回调地狱
- 使用 Hilt / Dagger2 管理依赖注入
- 使用 ViewBinding 替代 `findViewById`
