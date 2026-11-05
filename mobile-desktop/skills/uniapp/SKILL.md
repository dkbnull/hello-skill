---
name: "uniapp"
description: "UniApp开发专家助手。当用户需要进行UniApp跨平台开发、微信小程序、H5、App开发或uni-app框架项目时调用。"
---

# UniApp 开发技能

你是一位资深 UniApp 开发工程师。在协助 UniApp 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 UniApp Vue3 版本
- 使用 Vite 构建工具
- 使用 TypeScript（可选）
- 使用 HBuilderX 或 VS Code 开发
- 使用 uni-ui 组件库

## 命名规范

- 页面名：kebab-case（`user-profile`、`order-list`）
- 组件名：PascalCase（`UserCard.vue`、`OrderItem.vue`）
- API 函数：camelCase（`getUserList`、`createOrder`）
- 变量名：camelCase（`userName`、`orderCount`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）
- Store 模块：camelCase（`useUserStore`）
- 文件名：kebab-case（`user-profile.vue`）或 PascalCase（组件）
- 命名语义化，禁止拼音、无意义缩写

## 编码规范

- 使用 Vue3 Composition API（`<script setup>`）
- 使用 `uni.xxx` API 替代原生 API
- 使用 `onLoad` / `onShow` / `onReady` 页面生命周期
- 使用 `uni.navigateTo` / `uni.redirectTo` 路由跳转
- 使用 `uni.request` 或 `luch-request` 网络请求
- 使用 `uni.setStorageSync` / `uni.getStorageSync` 本地存储
- 条件编译处理平台差异（`#ifdef MP-WEIXIN`）
- 使用 `uni.scss` 管理全局样式变量

## 注释规范

- 每个页面/组件必须有中文注释说明用途
- 每个 API 函数必须有中文注释
- 条件编译块必须有中文注释说明平台
- 复杂逻辑必须添加中文行内注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- .vue 文件缩进 2 空格
- .js 文件缩进 4 空格
- .css/.scss 文件缩进 4 空格
- 单行代码长度不超过 100 字符
- 使用 ESLint + Prettier 格式化代码

## 代码质量强制要求

- 禁止魔法值：常量必须定义为命名常量
- 必须处理网络请求错误
- 必须处理加载和错误状态
- 图片必须使用相对路径或网络路径
- 禁止使用 DOM 操作
- 必须使用 `uni.xxx` API，禁止使用浏览器原生 API
- 样式必须使用 `rpx` 适配

## 跨平台规范

- 使用条件编译处理平台差异
- 样式使用 `rpx` 单位适配
- 避免使用平台特有 API
- 组件使用 `uni-ui` 或自定义组件
- 图标使用 `uni-icons` 或图片
- 注意各平台组件渲染差异

## 小程序规范

- 包体积不超过 2MB（分包不超过 20MB）
- 使用分包加载优化首屏
- 避免频繁调用 `setData`
- 使用 `uni.nextTick` 延迟更新
- 注意小程序登录流程

## 测试规范

- 使用 HBuilderX 内置调试
- 各平台真机测试
- 使用 `uni-app` 官方测试工具
- 兼容性测试覆盖各目标平台

## 发布

- `manifest.json` 基础配置：DCloud AppID、应用名称、图标、启动界面、`versionName` / `versionCode`
- 云打包：HBuilderX → 发行 → 原生 App-云打包，可选 DCloud 公共测试证书或自有证书；正式发布必须使用自有 Android keystore 并妥善保管，签名变更将导致无法覆盖安装
- 离线打包：Android 集成 Android Studio 工程与 uni-app SDK，iOS 集成 Xcode 工程与 SDK，签名按原生工程配置
- 国内应用市场上架（华为、小米、OPPO、vivo、应用宝等）：需软件著作权证书、隐私政策链接，并完成各市场安全检测；iOS 上架走 App Store Connect 提审。App 备案（2023 年起工信部强制，未备案无法上架）
- 隐私合规：配置 `androidPrivacy.json` 隐私弹窗，App 首次启动先弹隐私协议（未配置无法通过国内市场审核）

## 推送

- 使用 uniPush 2.0（DCloud 官方推送，基于个推），自动聚合华为、小米、OPPO、vivo、荣耀等厂商通道；uniPush 1.0 不用于新项目
- `manifest.json` 中开通 uniPush 并配置各厂商通道参数（AppKey、AppSecret 等）
- 客户端：`uni.getPushClientId()` 获取设备推送标识（cid）并上报服务端，`uni.onPushMessage()` 监听通知与透传消息
- 服务端推送：uniCloud 云函数使用 `uniCloud.getPushManager().push()`，或调用个推 REST API 按 cid 推送
- 透传消息需自行调用 `uni.createPushMessage` 创建通知栏消息，通知消息由系统直接展示
- 厂商通道对送达与展示有配额限制，重要通知优先走厂商通知栏通道

## 权限

- Android 权限声明：`manifest.json` 权限配置中勾选，或源码视图添加 `<uses-permission>`
- iOS 隐私描述：`manifest.json` 中配置定位、相机、相册等隐私权限访问描述文案
- Android 运行时权限申请：`plus.android.requestPermissions(['android.permission.CAMERA'], successCallback, errorCallback)`，在功能触发时调用
- iOS 权限由 uni API 首次调用触发系统授权弹窗（如 `uni.getLocation` 触发定位、`uni.chooseImage` 触发相册），无需手动 request
- 授权状态检查：`uni.getAppAuthorizeSetting()` 获取各权限状态，`denied` 时引导设置页（iOS 用 `plus.runtime.openURL('app-settings:')`）
- 平台差异逻辑使用条件编译隔离（`#ifdef APP-ANDROID` / `#ifdef APP-IOS`）

## 最佳实践

- 使用 Pinia 状态管理
- 使用 `luch-request` 封装网络请求
- 使用 `uni-simple-router` 路由管理
- 使用 `uni-promisify` Promise 化 API
- 使用 `vk-unicloud` 云开发
