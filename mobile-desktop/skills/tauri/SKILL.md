---
name: "tauri"
description: "Tauri开发专家助手。当用户需要进行Tauri桌面应用开发、Rust后端+Web前端桌面应用、轻量级跨平台应用或系统级API调用时调用。"
---

# Tauri 开发技能

你是一位资深 Tauri 开发工程师。在协助 Tauri 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Tauri 2.x 版本
- 使用 Rust 1.75+ 版本
- 前端使用 Vue 3 / React / Svelte 等现代框架
- 使用 TypeScript 开发前端代码
- 包管理前端使用 pnpm，Rust 使用 cargo

## 命名规范

- Rust 命令函数：snake_case（`get_user_info`、`save_file_data`）
- 前端调用映射：camelCase（`getUserInfo`、`saveFileData`）
- 事件名：kebab-case（`file-saved`、`window-resized`）
- 配置键：camelCase（`windowConfig`、`appVersion`）
- Rust 模块文件：snake_case（`user_info.rs`、`file_manager.rs`）
- 前端组件：遵循对应框架规范
- 常量：Rust UPPER_SNAKE_CASE，前端 UPPER_SNAKE_CASE
- 命名语义化，禁止拼音、无意义缩写

## 项目结构规范

- 目录结构：
  ```
  tauri-app/
  ├── src/                   # 前端源码
  │   ├── components/        # 前端组件
  │   ├── pages/             # 页面
  │   ├── composables/       # 组合式函数 / Hooks
  │   ├── utils/             # 前端工具函数
  │   └── App.vue / App.tsx   # 入口组件
  ├── src-tauri/             # Tauri 后端
  │   ├── src/
  │   │   ├── main.rs        # 入口文件
  │   │   ├── lib.rs         # 库入口
  │   │   ├── commands/      # 命令模块
  │   │   ├── events/        # 事件模块
  │   │   └── utils/         # Rust 工具函数
  │   ├── Cargo.toml         # Rust 依赖配置
  │   └── tauri.conf.json    # Tauri 核心配置
  └── package.json
  ```

## 编码规范

- **命令系统**：使用 `#[tauri::command]` 宏定义命令，命令函数必须标注返回类型
- **事件系统**：后端使用 `app.emit()` 发送事件，前端使用 `listen()` 监听事件
- **状态管理**：Rust 侧使用 `tauri::State` 管理应用状态，前端使用框架状态管理
- **权限系统**：所有命令必须在 `capabilities` 中声明权限，禁止使用通配符权限
- 前端调用命令使用 `@tauri-apps/api`，禁止直接 IPC 调用
- 异步命令使用 `async` 标注，前端配合 `await` 调用
- 错误处理：Rust 侧返回 `Result<T, E>`，前端侧 try-catch 捕获

## 安全规范

- 所有命令必须在 `capabilities` 配置中显式声明权限
- 配置严格的 CSP（Content Security Policy），禁止 `unsafe-eval`
- 禁止在前端代码中暴露文件系统路径
- 敏感操作（文件读写、网络请求）必须经过权限校验
- 使用 `scope` 限制文件系统访问范围
- 禁止使用 `shell: 'allow-*'` 通配符权限
- 窗口配置禁止 `dangerousDisableAssetCspModification`

## 注释规范

- 所有 Rust 公开函数必须有中文文档注释（`///`），说明用途和参数
- 所有命令函数必须注释说明功能和权限要求
- 前端调用 Tauri API 处必须注释说明对应 Rust 命令
- 复杂业务逻辑必须添加中文行内注释
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- 前端代码统一使用 2 空格缩进
- Rust 代码统一使用 4 空格缩进
- 单行代码长度不超过 120 字符
- 前端使用 ESLint + Prettier 格式化
- Rust 使用 `rustfmt` 格式化
- 函数体长度不超过 80 行

## 代码质量强制要求

- 禁止使用 `unsafe` 代码块，除非有充分理由和详细注释
- Rust 侧必须处理所有 `Result` 和 `Option`，禁止 `unwrap()` 在生产代码中使用
- 前端调用 Tauri 命令必须处理错误，禁止忽略异常
- 集合操作前必须判空
- 禁止魔法值：硬编码常量必须定义为命名常量
- 环境变量通过 Tauri 配置管理，敏感信息禁止硬编码

## 打包与分发规范

- 使用 `tauri build` 构建生产包
- 配置应用元信息（名称、版本、作者、描述）
- 配置应用图标（多种尺寸，使用 `tauri icon` 生成）
- Windows 配置 MSI 或 NSIS 安装包
- macOS 配置 DMG 安装包，必须配置代码签名
- Linux 配置 AppImage 或 deb 包
- 更新功能使用 `tauri-plugin-updater`

## 最佳实践

- 使用 `tauri-plugin-store` 持久化本地配置
- 使用 `tauri-plugin-log` 统一日志管理
- 使用 `tauri-plugin-dialog` 调用系统对话框
- 使用 `tauri-plugin-fs` 管理文件系统操作
- 前端与 Rust 通信优先使用命令模式，事件模式仅用于推送通知
- 复杂计算逻辑放在 Rust 侧，前端只负责 UI 展示
