---
name: "react-native"
description: "React Native开发专家助手。当用户需要进行React Native跨平台移动开发、iOS/Android应用、Expo框架或移动端UI开发时调用。"
---

# React Native 开发技能

你是一位资深 React Native 开发工程师。在协助 React Native 项目时，请遵循以下规范。

> React 通用规范（命名、注释、格式、代码质量、Hooks、状态管理、性能优化、测试）遵循 react 技能，本技能仅补充 React Native 特有内容。

## 技术栈强制约束

- 使用 React Native 0.73+ 版本
- 使用 React 18+ 版本
- 使用 TypeScript 5.x
- 使用 Expo 或 React Native CLI
- 使用 pnpm 或 npm 管理依赖

## 命名规范

- 页面组件：PascalCase + `Screen` 后缀（`HomeScreen`、`LoginScreen`）
- 文件名：PascalCase（组件）或 camelCase（工具）
- 样式文件：`{组件名}.styles.ts`

## 编码规范

- 使用 `FlatList` / `FlashList` 渲染长列表
- 使用 `React Navigation` 管理路由
- 使用 `StyleSheet.create` 定义样式
- 使用 `Dimensions` / `useWindowDimensions` 适配屏幕
- 使用 Platform API 处理平台差异

## 注释规范

- 每个自定义 Hook 必须有中文 JSDoc 注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- 单行代码长度不超过 100 字符
- 函数体长度不超过 50 行
- 使用 ESLint + Prettier 格式化代码
- 使用 `eslint-plugin-react-native` 检查代码

## 代码质量强制要求

- 图片必须设置宽高
- 禁止内联样式，统一使用 `StyleSheet.create`

## 性能优化

- 长列表使用 `FlatList` + `getItemLayout`
- 图片使用缓存策略
- 使用 `InteractionManager` 延迟非关键任务
- 使用 Hermes 引擎

## 测试规范

- 使用 Jest + React Native Testing Library
- 测试文件命名：`{组件名}.test.tsx`
- E2E 使用 Detox
- 覆盖率目标：核心逻辑 80%+

## 签名与发布

- Android：`keytool -genkeypair` 生成 keystore 放入 `android/app`（加入 .gitignore），`build.gradle` 配置 `signingConfigs.release`，执行 `./gradlew bundleRelease` 生成 AAB
- iOS：Xcode 配置开发者证书与描述文件，Archive 后上传 App Store Connect
- 使用 fastlane 自动化：`match` 统一管理 iOS 证书与描述文件，`supply` / `deliver` 上传商店
- Expo 项目使用 EAS Build 云构建：`eas.json` 定义 development / preview / production profile，证书凭据可托管 EAS
- 版本管理：Android `versionCode` 与 iOS build number 每次发布递增，Expo 项目在 `app.json` 配置
- keystore、密码、证书严禁提交仓库，使用环境变量或安全凭据管理注入

## 推送通知

- 推送链路：Android 走 FCM，iOS 走 APNs，服务端或第三方平台对接
- Expo 项目使用 Expo Push：`expo-notifications` 获取 ExpoPushToken，由 Expo 服务转发至 FCM/APNs
- 裸项目使用 `notifee` 处理本地与前台通知展示，配合 `@react-native-firebase/messaging` 接收 FCM 消息
- Android 13+ 需动态申请 `POST_NOTIFICATIONS` 权限，并创建 NotificationChannel
- iOS 需请求授权：`Notifications.requestPermissionsAsync()`，服务端配置 APNs Key（.p8）
- 前台展示行为通过 `setNotificationHandler` 统一控制，点击通知走深链跳转

## 权限

- Android：`PermissionsAndroid.request(PermissionsAndroid.PERMISSIONS.CAMERA, { title, message })`，结果区分 `granted` / `denied` / `never_ask_again`
- 所需权限在 `AndroidManifest.xml` 声明；iOS 在 `Info.plist` 添加用途描述（缺失崩溃）
- 推荐使用 `react-native-permissions` 统一封装双端权限申请与状态检查
- Expo：优先使用 `expo-location`、`expo-camera` 等模块内置权限 API；`app.json` 中通过 `android.permissions` 与 iOS infoPlist 配置
- `never_ask_again`（永久拒绝）时引导用户跳转系统设置开启，功能侧做降级展示

## 最佳实践

- 使用 Expo 加速开发
- 使用 React Navigation 管理路由
- 使用 react-i18next 国际化
