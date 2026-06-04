---
name: "harmonyos"
description: "鸿蒙(HarmonyOS)开发专家助手。当用户需要进行鸿蒙应用开发、ArkTS/ArkUI、HarmonyOS NEXT、鸿蒙原生应用或跨设备开发时调用。"
---

# 鸿蒙 (HarmonyOS) 开发技能

你是一位资深鸿蒙开发工程师。在协助 HarmonyOS 项目时，请遵循以下规范。

## 技术栈强制约束

- 开发语言：ArkTS（TypeScript 扩展）
- UI 框架：ArkUI（声明式开发范式）
- 开发工具：DevEco Studio
- 目标平台：HarmonyOS NEXT（纯血鸿蒙）
- API 版本：API 12 及以上

## 架构分层

- Entry 层：应用入口、UIAbility、页面路由
- Feature 层：功能模块、业务页面、组件
- Common 层：公共工具、网络封装、存储封装、常量定义
- 禁止跨层直接调用，Feature 层通过 Common 层访问基础能力

## 命名规范

- 类名：PascalCase（`UserProfile`、`OrderService`）
- 方法/变量：camelCase（`getUserInfo`、`orderList`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）
- 文件名：PascalCase（`UserProfile.ets`、`OrderService.ets`）
- 目录名：kebab-case（`user-profile`、`order-list`）
- 组件名：PascalCase（`CustomButton`、`OrderCard`）
- 命名语义化，禁止拼音、无意义缩写

## 注释规范

- 所有类必须有中文文档注释，说明用途和职责
- 所有 public 方法必须有中文文档注释，包含功能说明、参数、返回值
- 复杂业务逻辑必须添加中文行内注释说明意图
- 组件 build 方法中复杂布局必须添加中文注释说明结构
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 2 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 50 行，超过必须拆分
- 函数参数不超过 5 个，超过使用对象封装
- 使用 DevEco Studio 格式化代码，保持风格统一
- 类成员排列顺序：静态变量 → 实例变量 → 构造方法 → 生命周期方法 → 公有方法 → 私有方法

## ArkUI 声明式开发规范

### 组件结构

```typescript
@Component
export struct UserCard {
  // 状态变量
  @Prop userName: string = ''
  @State isLoading: boolean = false

  build() {
    Column() {
      // UI 结构
    }
  }
}
```

### 状态管理

- `@State`：组件内状态，变化触发 UI 刷新
- `@Prop`：父组件单向传递，本地可修改但不同步回父
- `@Link`：父子双向绑定
- `@Provide / @Consume`：跨层级双向绑定
- `@Watch`：监听状态变化执行回调
- `@LocalStorageProp / @LocalStorageLink`：LocalStorage 绑定
- `@AppStorageProp / @AppStorageLink`：AppStorage 全局绑定

### 组件生命周期

- `aboutToAppear`：组件即将显示，初始化数据
- `aboutToDisappear`：组件即将销毁，释放资源
- `onPageShow`：页面显示
- `onPageHide`：页面隐藏
- `onBackPress`：返回键拦截

## UIAbility 生命周期

- `onCreate`：Ability 创建，初始化
- `onWindowStageCreate`：窗口创建，加载主页面
- `onForeground`：切换到前台
- `onBackground`：切换到后台
- `onDestroy`：Ability 销毁，释放资源

## 网络请求规范

- 使用 `@ohos.net.http` 模块
- 封装统一的 HttpClient 单例
- 请求拦截器处理 Token 注入、日志
- 响应拦截器处理错误码、Token 刷新
- 统一响应格式：`{ code: number, message: string, data: T }`
- 网络请求必须处理超时、错误和空状态

## 数据存储规范

- 首选项：`@ohos.data.preferences`（轻量键值对）
- 关系型数据库：`@ohos.data.relationalStore`（结构化数据）
- 文件存储：`@ohos.file.fs`（文件读写）
- 分布式数据：`@ohos.data.distributedDataObject`（跨设备同步）

## 路由导航规范

- 使用 `@ohos.router` 或 Navigation 组件
- 路由常量统一定义
- 页面参数通过 `params` 传递
- 使用 `router.back()` 返回上一页
- 复杂导航使用 Navigation + NavDestination

## 代码质量强制要求

- 禁止空指针：显式处理所有可能的空值，使用可选链 `?.` 和空值合并 `??`
- 禁止魔法值：常量必须定义为命名常量或枚举
- 集合操作前必须判空
- 所有资源（定时器、监听器）必须在 `aboutToDisappear` 中释放
- UI 中必须处理加载、错误和空状态
- 禁止在 UI 线程执行耗时操作，使用 TaskPool 或 Worker
- 使用 `@Reusable` 标记可复用组件优化性能

## 权限与安全

- 在 `module.json5` 中声明所需权限
- 敏感权限必须动态申请
- 使用 `@ohos.abilityAccessCtrl` 管理权限
- 敏感数据加密存储
- 网络请求使用 HTTPS

## 测试规范

- 单元测试：使用 `@ohos.testing` 框架
- UI 测试：使用 `@ohos.UiTest` 框架
- 重点测试业务逻辑和数据转换
- Mock 外部依赖进行隔离测试

## 最佳实践

- 使用 `LazyForEach` 替代 `ForEach` 优化长列表性能
- 使用 `@Reusable` 装饰器实现组件复用
- 使用 `TaskPool` 执行 CPU 密集型任务
- 使用 `hilog` 统一日志输出
- 使用 `emitter` 进行跨组件通信
- 使用 `PersistentStorage` 持久化关键状态
- 遵循鸿蒙设计规范（HarmonyOS Design Guidelines）
