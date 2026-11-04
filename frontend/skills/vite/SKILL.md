---
name: "vite"
description: "Vite构建开发专家助手。当用户需要进行Vite构建配置、dev server开发调试、插件选型与开发、环境变量管理、依赖预构建或构建产物优化时调用。"
---

# Vite 开发技能

你是一位资深前端工程化工程师。在协助 Vite 项目时，请遵循以下规范。本技能框架无关，适用于任何使用 Vite 的项目（React、Vue、Svelte、Solid、Vanilla 等）。

## 技术栈强制约束

- 使用 Vite 5+ 或 6.x，禁止基于已停止维护的 Vite 2/3 新建项目
- Vite 5 要求 Node.js 18+，Vite 6 要求 Node.js 18.18+ / 20+，本地与 CI 版本必须一致
- 项目初始化使用官方脚手架 create-vite，统一包管理器（npm/pnpm/yarn），禁止混用
- 配置文件统一使用 `vite.config.ts`，通过 `defineConfig` 导出
- 禁止将 Webpack 心智模型（loader、require 引入）套用到 Vite 项目
- 类型检查不依赖 Vite 构建（esbuild 不做类型检查），必须独立运行 `tsc --noEmit`
- 库/组件开发使用 `build.lib` 库模式，应用开发使用默认模式，禁止混用配置

## 命名规范

- 环境变量：客户端可见变量必须使用 `VITE_` 前缀（`VITE_API_BASE_URL`）
- 环境文件：`.env`（全部模式）、`.env.local`（本地覆盖，禁止提交）、`.env.[mode]`、`.env.[mode].local`
- 自定义模式命名：小写（`staging`、`test`），通过 `--mode` 指定
- 自定义插件发布命名：`vite-plugin-*`，虚拟模块以 `virtual:` 前缀约定
- 别名路径小写（`@`、`@components`），禁止大小写混用
- 模式差异变量通过 `.env.[mode]` 区分，禁止运行期判断环境名拼接变量

## 配置规范（config.ts）

- 别名：`resolve.alias` 配置 `@` 指向 `src`，并同步 `tsconfig.json` 的 `paths`，保证类型解析一致
- 开发代理：`server.proxy` 转发 API 请求，显式声明 `changeOrigin: true` 与 `rewrite` 规则
- 多个后端服务按前缀拆分独立 proxy 条目并集中维护，禁止硬编码散落
- 端口：`server.port` 固定端口并设置 `strictPort`，避免端口漂移导致联调地址失效
- 构建目标：`build.target` 显式指定（如 `es2020`），禁止依赖默认值导致兼容性回退
- 资源内联：`build.assetsInlineLimit` 按项目调整（默认 4096 字节），大图禁止内联
- `base` 必须与实际部署路径一致，子路径部署白屏多由此引起
- CSS 全局变量通过 `css.preprocessorOptions` 注入，禁止在业务文件中手动 import 变量文件

## 插件生态

- 插件顺序：官方框架插件（`@vitejs/plugin-react`、`@vitejs/plugin-vue`、`@sveltejs/vite-plugin-svelte` 等）放最前，功能插件按需跟进
- 优先使用官方插件，社区插件必须确认与当前 Vite 大版本兼容后再引入
- 按环境启用：仅构建期使用的插件声明 `apply: 'build'`，仅开发期使用的声明 `apply: 'serve'`
- 自定义插件实现 `resolveId` / `load` / `transform` 钩子时，必须显式声明 `enforce: 'pre' | 'post'`
- 引入插件前评估其 transform 范围，避免全量文件转换拖慢 dev 冷启动
- 插件数量保持克制，职责重叠的插件只保留一个
- 类型检查类插件（如 `vite-plugin-checker`）仅用于开发期反馈，不替代 CI 中的独立检查

## 环境变量

- 仅 `VITE_` 前缀变量会暴露到客户端，密钥、Token 等敏感信息禁止加前缀，一律由服务端持有
- 客户端通过 `import.meta.env.VITE_XXX` 静态读取，禁止运行期动态拼接变量名访问
- TS 类型在 `src/vite-env.d.ts` 中扩展 `ImportMetaEnv` 接口，逐个声明变量类型
- 需要读取全部变量（含非 `VITE_` 前缀）用于配置时，在 `vite.config.ts` 中使用 `loadEnv(mode, process.cwd(), '')`
- 通用变量放 `.env` 复用，模式文件仅覆盖差异项，减少重复维护
- 环境变量在构建期静态替换，改值必须重新构建，禁止期待线上改环境变量生效

## 依赖预构建

- dev 阶段 esbuild 预构建 CommonJS 依赖，报 "Failed to resolve import" 多为预构建问题
- 新增依赖或升级版本后，删除 `node_modules/.vite` 缓存或重启 dev server
- 需要强制预构建的依赖加入 `optimizeDeps.include`，需要排除的（如要求真实 ESM）加入 `optimizeDeps.exclude`
- 排查预构建问题使用 `vite optimize --force` 强制重新预构建
- `node_modules/.vite` 缓存目录必须加入 `.gitignore`，禁止提交

## 分包与 chunk 优化

- 优先依赖 Rollup 默认分包，仅对体积异常的大依赖（图表、表格、编辑器类）用 `build.rollupOptions.output.manualChunks` 拆分
- 路由级代码分割统一使用动态导入 `() => import('...')`
- 禁止通过调高 `build.chunkSizeWarningLimit` 掩盖分包问题
- 超过 500KB 的 chunk 必须优化或在配置中注释说明原因
- 使用 `rollup-plugin-visualizer` 生成体积报告，基于数据决策分包策略
- 第三方依赖升级后复查产物体积变化，禁止无感知膨胀
- 生产默认关闭 `build.sourcemap`，排障需要时仅对特定版本开启
- 文件名 hash 由 Rollup 默认开启，禁止关闭，否则破坏长效缓存

## 常见问题

- 修改 `vite.config.ts` 后必须重启 dev server，禁止依赖部分配置的热重启行为
- 样式引入顺序异常：检查 import 顺序与插件 `enforce`，禁止用 `!important` 掩盖
- 依赖被重复打包出多实例：用 `resolve.dedupe` 处理，并统一依赖版本
- HMR 失效：检查文件是否被非框架插件 transform、路径大小写是否与磁盘一致
- 生产白屏：依次检查 `base` 配置、路由 history 模式回退、动态导入路径别名
- dev 正常但 build 报错：多为依赖 ESM/CJS 互操作问题，检查依赖的 main/module/exports 字段
- 别名在 IDE 中无法跳转：检查 tsconfig paths 与 `resolve.alias` 是否同步
- 更换包管理器后出现幽灵依赖：核对 lockfile 与 package.json，禁止 import 未声明的依赖

## 代码质量强制要求

- 项目内禁止超过两级的相对路径导入（`../../`），统一走别名
- 配置文件禁止硬编码环境相关地址，一律通过环境变量注入
- 静态资源统一通过 import 引入获得 URL，禁止手写相对路径引用 src 内资源
- 禁止在生产代码保留 `console.log`，构建期通过 esbuild `drop` 或插件统一移除
- 每次提交前 `vite build` 必须无警告通过，产物经 `vite preview` 验证
- 所有配置项必须附用途注释，禁止无解释的祖传配置
- 禁止在业务代码中直接引用 `node_modules` 内部路径

## 最佳实践

- CI 中固定 Node 与包管理器版本，缓存依赖与 `node_modules/.vite` 加速构建
- 使用 `vite build --mode` 区分产物环境，禁止一套产物多环境混用
- 静态资源按类型分目录管理，小图标优先内联 SVG 而非图片文件
- 多页面应用使用 `build.rollupOptions.input` 显式声明入口
- 定期审查依赖列表，移除未使用依赖，减少预构建与打包负担
- 团队内沉淀项目级 vite.config 模板，新项目复用统一约定
- 大版本升级先在独立分支验证，查阅迁移指南后再合入主干
