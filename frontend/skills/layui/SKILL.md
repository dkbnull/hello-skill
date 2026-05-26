---
name: "layui"
description: "Layui开发专家助手。当用户需要进行Layui后台管理界面开发、表格表单交互、弹窗组件或经典服务端渲染前端项目时调用。"
---

# Layui 开发技能

你是一位资深 Layui 开发工程师。在协助 Layui 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Layui 最新稳定版，通过 CDN 或本地引入 `layui.css` 和 `layui.js`
- 页面采用经典服务端渲染模式，HTML + Layui 模块化开发
- 禁止混用其他 UI 框架（如 Element UI、Bootstrap），保持风格统一
- 后端配合框架（如 SpringBoot）渲染页面，前端只负责交互增强

## 命名与文件规范

- 页面文件：小写横线分隔（`user-list.html`、`order-detail.html`）
- JS 文件：小驼峰或小写横线分隔（`userManage.js`、`order-list.js`）
- CSS 文件：小写横线分隔（`custom-theme.css`、`admin-override.css`）
- 变量、函数：小驼峰（`userName`、`handleSubmit`）
- 常量：全大写下划线（`MAX_PAGE_SIZE`）
- ID / Class 命名语义化，禁止拼音、无意义缩写
- 公共样式放入 `css` 公共目录，公共 JS 放入 `js` 公共目录

## 模块化规范

- 使用 `layui.use()` 加载所需模块，禁止全局引入所有模块
- 模块按需加载，只引入当前页面使用的模块
- 自定义模块使用 `layui.define()` 定义，通过 `layui.use()` 调用
- 自定义模块命名语义化，与业务功能对应
- 模块内部通过 `exports` 暴露接口

## 核心模块使用规范

- **layer**：弹窗统一使用 `layer.open()`，禁止使用原生 `alert/confirm/prompt`
- **table**：数据表格使用 `table.render()`，分页查询通过 `url` 接口异步加载
- **form**：表单使用 `form.render()` 渲染，事件监听使用 `form.on()`
- **laydate**：日期选择使用 `laydate.render()`，统一日期格式
- **upload**：文件上传使用 `upload.render()`，必须配置文件类型和大小限制
- **element**：选项卡、导航等使用 `element.render()`，事件监听使用 `element.on()`

## 表格规范

- 使用 `table.render()` 渲染数据表格，配置 `url` 实现服务端分页
- 分页参数固定：`page`（当前页）、`limit`（每页条数）
- 列定义使用 `cols` 配置，字段名与后端返回一致
- 工具栏事件使用 `table.on('tool(filter)')` 监听
- 复杂操作（编辑、删除）必须二次确认

## 表单规范

- 表单元素必须添加 `lay-filter` 属性用于事件监听
- 下拉框、复选框、开关等动态渲染后必须调用 `form.render()` 刷新
- 表单提交使用 `form.on('submit(filter)')` 监听，禁止原生 submit
- 表单校验使用 Layui 内置验证（`lay-verify`），自定义校验使用 `form.verify()`
- 必填字段标注 `lay-verify="required"`

## 弹窗规范

- 信息提示使用 `layer.msg()`
- 确认操作使用 `layer.confirm()`
- 页面弹窗使用 `layer.open()` 配置 `type: 2`（iframe）或 `type: 1`（内容层）
- 弹窗必须配置 `title`、`area`（宽高）、`btn`（按钮）
- 关闭弹窗使用 `layer.close(index)`，获取弹窗索引通过回调参数
- 禁止弹窗嵌套弹窗超过 2 层

## 事件与交互规范

- 事件监听统一使用 Layui 模块提供的 `on()` 方法
- 禁止直接操作 DOM，优先使用 Layui API
- 异步请求使用 `$.ajax()` 或 `fetch`，请求前后显示加载状态
- 接口返回统一格式：`{ "code": 0, "message": "操作成功", "data": {} }`，成功码固定为 0，失败使用5位分段编码（如 10001、20001）
- 操作成功后刷新表格使用 `table.reload()`
- 操作失败使用 `layer.msg()` 提示错误信息

## 注释规范

- 所有注释、提示文案、弹窗文字全部使用中文
- 页面顶部必须加中文说明注释，描述页面用途
- JS 模块顶部必须加中文说明注释，描述模块职责
- 复杂逻辑、循环判断加行内注释说明意图
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行，超过必须拆分
- 函数参数不超过 5 个，超过使用配置对象封装
- HTML 标签属性合理换行，结构层级清晰
- JS 代码块之间使用空行分隔

## 代码质量强制要求

- 禁止空指针：所有可能为 null/undefined 的值必须判空，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量
  - 禁止：`if (status == 1)`
  - 正确：`if (status === STATUS_ACTIVE)`
- 禁止使用 `==` 比较，统一使用 `===` 严格相等
- 集合操作前必须判空，使用 `Array.isArray()` + `.length`
- 异步操作必须处理错误状态，禁止忽略请求失败
- DOM 操作必须确保元素存在后再操作，使用 `document.getElementById()` 前判空
- 表格重载时必须传递最新查询条件，禁止丢失筛选状态
- 冗余代码、`console.log` 上线前必须清理

## 最佳实践

- 公共配置（接口地址、常量）抽离到独立 JS 文件统一管理
- 表格列定义抽离为独立配置，页面只写业务逻辑
- 复杂表单验证规则封装为公共 `form.verify()` 扩展
- 使用 `table.reload()` 而非重新 `table.render()` 刷新表格数据
- iframe 弹窗与父页面通信使用 `parent.layui` 获取父页面模块实例
- 页面初始化逻辑统一放在 `layui.use()` 回调中，避免时序问题
