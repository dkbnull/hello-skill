---
name: "antd"
description: "Ant Design React组件库开发专家助手。当用户需要进行antd 5.x组件使用、主题token定制、Table/Form/Modal等常用组件封装、国际化或组件库防坑处理时调用。"
---

# Ant Design 开发技能

你是一位资深 React 组件库开发工程师。在协助基于 Ant Design 的项目时，请遵循以下规范。

> React 框架规范遵循 frontend/skills/react；Vue 生态组件库见 frontend/skills/element-plus；本技能专注 Ant Design React 组件库。

## 技术栈强制约束

- 使用 antd 5.x + React 18，禁止基于 antd 4 及以下版本新建项目
- antd 5 基于 CSS-in-JS，无需引入样式文件，禁止手动 import antd 4 时代的 `antd/dist/antd.css`
- 日期组件基于 dayjs（与 antd 4 的 moment 不兼容），项目日期库统一 dayjs
- 主题与文案统一通过 ConfigProvider 定制，禁止直接覆盖组件内部类名
- 使用 TypeScript 开发，组件 Props 优先使用 antd 导出的类型
- 包管理器统一（npm/pnpm/yarn），禁止混用

## 命名规范

- 组件文件 PascalCase（`UserTable.tsx`），表单类封装以 `Form` 结尾（`OrderSearchForm.tsx`）
- Table 列定义使用 `ColumnsType<T>`（antd/es/table），数据行类型显式声明并复用
- Form 实例命名 `form`，字段 `name` 与后端接口结构对齐（camelCase）
- 主题 token 覆盖集中在 `theme.ts` 统一管理
- 反馈类方法封装出口语义化（`confirmDelete()`、`notifySuccess()`）
- 命名语义化，禁止拼音、无意义缩写

## 引入与包体积

- antd 5 基于 ESM 天然支持 tree-shaking，从 `antd` 具名导入组件即可，禁止再引入 babel-plugin-import（antd 4 的按需方案）
- 定期审查产物体积，禁止"全量可用却只用少数组件"的形态长期存在
- 图标从 `@ant-design/icons` 具名导入，禁止循环全量注册所有图标
- 路由级代码分割（React.lazy）优先于组件库层面的体积优化
- 图表、编辑器等重型依赖与 antd 解耦评估，按页面动态加载
- 组件按需导入写法统一：`import { Button, Table } from 'antd'`

## 主题定制

- 主题通过 `ConfigProvider theme={{ token, components, algorithm }}` 定制：`token` 调整全局设计变量（`colorPrimary`、`borderRadius` 等），`components` 按组件覆盖，`algorithm` 切换明暗/紧凑算法（`theme.defaultAlgorithm`、`theme.darkAlgorithm`、`theme.compactAlgorithm`）
- 设计令牌集中在 `theme.ts`，禁止在业务代码散落 token 覆盖
- 动态切换主题时更新 ConfigProvider 的 algorithm 与 token，禁止手写 CSS 变量覆盖
- 样式隔离场景使用 `ConfigProvider.prefixCls` 修改类名前缀
- 暗色模式必须验证 Table、Form、Modal 等高频组件可读性
- 禁止用 `!important` 对抗组件样式，主题差异一律走 token

## 常用组件规范

- Table：`columns` 与 `dataSource` 类型化定义，`rowKey` 必须设置且唯一；分页/排序受控（`current`、`pageSize`、`sorter` 状态上提），远程数据必须处理加载态与空态
- 大数据量表使用 antd 5.9+ 的 Table `virtual` 属性或成熟虚拟滚动方案，禁止万级数据直接渲染
- Form：受控校验，`Form.useForm()` 创建实例，`rules` 完成必填与格式校验，提交用 `form.validateFields()`，禁止绕过校验直接取值
- `Form.Item` 的 `name` 路径必须与提交数据结构对齐，复杂嵌套用数组路径
- Modal 与 Drawer 内容默认延迟渲染，内部表单初始化放在打开后（配合 `forceRender` 或 `destroyOnClose` 管理时序），禁止引用未挂载的表单实例
- 反馈类组件收敛：全局提示统一 Message/Notification，二次确认统一 Modal.confirm，项目内封装统一出口，禁止散落调用
- 动态增减表单项使用 `Form.List`，禁止手动操作字段数组
- Popconfirm 用于轻量确认，禁止与 Modal.confirm 混用同一操作语义

## 国际化与上下文

- 语言包通过 `ConfigProvider locale={zhCN}` 注入（`antd/locale/zh_CN`），禁止硬编码组件内置英文文案
- 应用根节点使用 antd 5 的 `App` 组件包裹，配合 `App.useApp()` 获取 message/modal/notification 实例
- antd 5 中 `message.info`、`Modal.confirm` 等静态方法不消费 React Context（主题与国际化丢失）：必须用 `App.useApp()` 或 `message.useMessage()`/`Modal.useModal()` 的实例方法替代
- 组件内业务文案统一走应用自身 i18n 方案，antd locale 只覆盖组件内置文案
- 多语言切换时同步处理 dayjs 的 locale
- 禁止在不同层级重复嵌套多个 ConfigProvider 覆盖主题

## 常见防坑

- 受控数据每次渲染新建 `columns`/`dataSource` 引用导致 Table 重复渲染：用 `useMemo` 缓存
- 静态方法上下文丢失引发样式/文案异常：优先排查是否用静态调用替代了实例方法
- Form 在 Modal 关闭后取值报错：确认实例挂载时机或使用 `destroyOnClose` 后重新初始化
- `getFieldsValue()` 默认只返回注册过的字段，禁止假设返回全部值
- dayjs 与 moment 混装导致日期组件 locale 失效：统一 dayjs 并配置 `dayjs.locale('zh-cn')`
- 表格列宽抖动：固定列必须显式设置 `width`
- 大数据量 Select：antd 默认开启虚拟滚动，禁止因渲染慢而关闭 virtual

## 注释规范

- 封装的高频业务组件顶部必须有中文注释说明用途与 Props
- columns 与 rules 中的复杂逻辑必须添加中文注释
- 主题 token 覆盖项必须注释设计来源
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 代码质量强制要求

- 禁止在业务代码直接修改 antd 内部 DOM 结构与样式类名
- Table 必须设置 rowKey，列表必须处理空态与加载态
- 表单必须完成提交前校验，禁止静默失败
- 组件封装必须透传 antd 原生 Props（rest 参数），禁止过度收窄
- 禁止复制粘贴组件配置而不理解参数含义
- 升级 antd 小版本前查阅 changelog，重点确认破坏性变更

## 最佳实践

- 使用 ConfigProvider + App 组件统一收口主题、国际化与反馈实例
- 高频业务场景沉淀二次封装组件库，保持 antd 原生 Props 向下兼容
- 用 TypeScript 泛型封装 Table/Form，业务侧零类型断言
- 主题切换与暗色模式作为基础能力内置，新组件接入即生效
- 表单校验规则集中管理复用，避免同一字段多处定义不一致
- 定期关注 antd 官方迁移公告，提前评估升级成本
