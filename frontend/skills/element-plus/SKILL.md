---
name: "element-plus"
description: "Element Plus Vue 3组件库开发专家助手。当用户需要进行Element Plus组件使用、按需自动导入、主题CSS变量定制、el-table/el-form/el-dialog等常用组件封装或组件库防坑处理时调用。"
---

# Element Plus 开发技能

你是一位资深 Vue 3 组件库开发工程师。在协助基于 Element Plus 的项目时，请遵循以下规范。

> Vue 框架规范遵循 frontend/skills/vue；React 生态组件库见 frontend/skills/antd；本技能专注 Element Plus（Vue 3）组件库。

## 技术栈强制约束

- 使用 Element Plus + Vue 3 组合式 API（`<script setup>`），禁止在项目中混用 Vue 2 语法
- 引入方式二选一并全项目统一：全量引入（`app.use(ElementPlus)`）或按需自动导入（unplugin-vue-components + unplugin-auto-import 的 ElementPlusResolver），禁止混用
- 按需导入时组件样式由 resolver 自动注入，禁止手动逐个 import 样式文件
- 主题与文案统一通过 ElConfigProvider 或全局配置，禁止直接覆盖组件内部样式类
- 使用 TypeScript 开发，组件 Props 优先使用 Element Plus 导出的类型
- 构建工具使用 Vite，与 Element Plus 官方生态保持一致
- 包管理器统一（推荐 pnpm），禁止混用

## 命名规范

- 封装组件文件 PascalCase（`UserTable.vue`），模板标签风格全项目统一（PascalCase 或 kebab-case）
- 表单字段 `prop` 命名与提交数据结构一致（camelCase）
- 主题 CSS 变量覆盖集中在样式入口文件统一管理
- 反馈类方法封装出口语义化（`confirmDelete()`、`notifySuccess()`）
- 命名语义化，禁止拼音、无意义缩写

## 引入方式

- 全量引入适合中小型中后台项目：`app.use(ElementPlus)` 一次注册全部组件
- 中大型项目使用按需自动导入：unplugin-vue-components + unplugin-auto-import 配合 ElementPlusResolver，自动完成组件与样式的按需引入
- 按需导入时 ElMessage/ElMessageBox/ElNotification 等函数式 API 需通过 auto-import 配置或显式 import 引入，并确保对应样式被注入
- 禁止同一项目内既有全量 `use` 又有按需 resolver，避免样式注入顺序冲突
- 体积异常时用构建分析复核 Element Plus 贡献占比

## 主题定制

- Element Plus 通过 `--el-*` CSS 变量命名空间暴露设计变量，定制统一覆盖 CSS 变量（如 `--el-color-primary` 及其 light/dark 色阶），禁止硬编码覆盖组件内部选择器
- 主题色色阶用官方在线工具或 SCSS 方式（`@use "element-plus/theme-chalk/src/index.scss" with (...)`）生成，保持变量层级完整
- 暗色模式：引入 `element-plus/theme-chalk/dark/css-vars.css`，给 `html` 添加 `dark` 类切换，业务色板需同步适配暗色
- 局部主题用 `ElConfigProvider` 或页面级容器包裹覆盖变量，禁止全局与局部混用导致优先级混乱
- 暗色模式下必须验证表格、表单、弹窗可读性
- 自定义业务组件的间距、圆角优先复用 `--el-*` 变量，保持视觉一致
- 禁止用 `!important` 对抗组件样式，主题差异一律走 CSS 变量

## 常用组件规范

- el-table：列用 `el-table-column` 声明，自定义单元格使用作用域插槽（`#default="{ row }"`）；分页受控（`v-model:current-page`、`v-model:page-size`），远程数据必须处理 loading 与空态；`row-key` 必须设置且唯一
- 大数据量表使用 el-table-v2（虚拟化表格），大数据下拉使用 el-select-v2（虚拟化选择器），禁止万级数据直接渲染
- el-form：`model` + `prop` + `rules` 声明式校验，提交用 `formRef.validate()`，重置用 `resetFields()`；自定义 validator 必须保证所有分支都回调
- `resetFields()` 只重置到表单初始值，`prop` 未正确设置时无效，编辑回显场景按需手动清空
- el-dialog / el-drawer：受控 `v-model` 控制显隐，禁止手动操作 DOM；弹窗内表单初始化在打开后进行（`nextTick` 或 `destroy-on-close`），禁止引用未挂载的表单实例
- 反馈类组件收敛：轻提示 ElMessage、确认框 ElMessageBox、通知 ElNotification 统一封装出口，禁止业务代码散落调用
- el-upload：自定义上传用 `http-request`，文件列表受控（`v-model:file-list`），大文件上传必须提供进度与取消能力
- 同一操作的确认交互只保留一种模式（MessageBox 或 Popconfirm）

## 国际化与图标

- 语言包通过 `ElConfigProvider locale` 或全局 `app.use(ElementPlus, { locale })` 注入（`element-plus/es/locale/lang/zh-cn`），禁止硬编码组件内置英文文案
- 图标使用 `@element-plus/icons-vue`，按需导入后配合 `<el-icon>` 使用
- 全局注册图标时逐个注册所需图标，禁止循环全量注册
- 图标组件命名与 Element Plus 导出名保持一致，禁止二次重命名造成检索困难
- 应用内业务文案统一走自身 i18n 方案，Element Plus locale 只覆盖组件内置文案
- 组件尺寸通过 `ElConfigProvider size` 全局统一，个别组件按需覆盖，禁止逐个散落设置

## 常见防坑

- ElMessage/ElMessageBox 等函数式调用脱离组件树，无法消费 `el-config-provider` 提供的上下文（局部国际化、z-index 等），需要全局一致行为时通过全局配置或统一封装处理
- 弹窗内表单校验时序：dialog 默认懒渲染，表单未挂载时调用 `resetFields()`/`validate()` 会报错，注意初始化时机
- `resetFields()` 重置的是初始值而非空值，编辑场景回显后重置会回到回显值
- 受控数据每次渲染新建引用导致 el-table 重复渲染：用 `computed`/`ref` 缓存
- 表格固定列与展开行同时使用出现错位时，保持 Element Plus 版本更新并验证
- 表单规则改动后必须重新校验，禁止依赖旧校验状态

## 注释规范

- 封装的高频业务组件顶部必须有中文注释说明用途与 Props
- rules 与作用域插槽中的复杂逻辑必须添加中文注释
- 主题 CSS 变量覆盖项必须注释用途
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 代码质量强制要求

- 禁止通过 `!important` 或深层选择器覆盖组件内部样式，主题差异一律走 CSS 变量
- el-table 必须设置 row-key，列表必须处理空态与加载态
- 表单必须完成提交前校验，禁止静默失败
- 封装组件必须透传原生 Props 与插槽，禁止过度收窄
- 禁止在模板中编写复杂业务逻辑，收敛到组合式函数
- 升级 Element Plus 版本前查阅 changelog，重点确认破坏性变更

## 最佳实践

- 中大型项目固定使用按需自动导入，并定期复核产物体积
- 高频业务场景沉淀二次封装组件，保持 Element Plus 原生能力向下兼容
- 表格 + 分页 + 搜索组合封装为标准列表组件，新页面直接复用
- 暗色模式与主题切换作为基础能力内置，新组件接入即生效
- 表单校验规则集中管理复用，避免同一字段多处定义不一致
- 大版本升级先在独立分支回归验证，再合入主干
