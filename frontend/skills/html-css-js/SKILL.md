---
name: "html-css-js"
description: "HTML/CSS/JavaScript开发专家助手。当用户需要进行网页开发、响应式布局、CSS动画、原生JS交互或Web前端基础开发时调用。"
---

# HTML / CSS / JavaScript 开发技能

你是一位资深 Web 前端开发工程师。在协助 HTML/CSS/JS 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 HTML5 语义化标签
- 使用 CSS3 + CSS 自定义属性（CSS Variables）
- 使用 ES6+ 语法（箭头函数、解构、模板字符串、Promise、async/await）
- 响应式设计优先使用 CSS Grid + Flexbox
- 禁止使用 jQuery，原生 API 已足够成熟

## 命名规范

- HTML 文件：小写 + 短横线分隔（`user-list.html`、`order-detail.html`）
- CSS 文件：小写 + 短横线分隔（`custom-theme.css`、`admin-override.css`）
- JS 文件：小写 + 短横线分隔（`user-service.js`、`order-handler.js`）
- CSS 类名：BEM 命名或小写 + 短横线分隔
  - BEM：`block__element--modifier`（`user-card__title--active`）
  - 短横线：`user-card-title`、`btn-primary`
- CSS 变量：`--{命名空间}-{属性}`（`--color-primary`、`--spacing-md`）
- JS 变量/函数：camelCase（`userName`、`handleSubmit`）
- JS 常量：UPPER_SNAKE_CASE（`MAX_PAGE_SIZE`、`API_BASE_URL`）
- JS 类名：PascalCase（`UserService`、`OrderValidator`）
- ID 命名：小写 + 短横线分隔（`user-list`、`order-form`）
- 命名语义化，禁止拼音、无意义缩写

## HTML 规范

- 使用语义化标签：`<header>`、`<nav>`、`<main>`、`<article>`、`<section>`、`<aside>`、`<footer>`
- 禁止滥用 `<div>` 和 `<span>`
- 表单元素必须关联 `<label>`
- 图片必须添加 `alt` 属性
- 链接必须添加有意义的链接文本
- 属性顺序：`id` → `class` → `data-*` → `src/href` → `title/alt`
- 自闭合标签不加斜杠：`<br>`、`<img>`、`<input>`
- 缩进 2 空格

## CSS 规范

- 使用 CSS 自定义属性定义主题变量：
  ```css
  :root {
    --color-primary: #1890ff;
    --color-success: #52c41a;
    --spacing-md: 16px;
    --font-size-base: 14px;
  }
  ```
- 布局优先使用 CSS Grid + Flexbox，禁止 float 布局
- 响应式设计使用媒体查询 + 相对单位（rem、em、vw、vh）
- 移动优先策略：先写移动端样式，再通过 `min-width` 扩展
- 选择器嵌套层级不超过 3 层
- 禁止使用 `!important`，通过提高选择器优先级解决
- 动画优先使用 `transform` 和 `opacity`，避免触发重排
- CSS 属性声明顺序：
  1. 定位（position、top、left）
  2. 盒模型（display、width、margin、padding）
  3. 排版（font、line-height、text-align）
  4. 视觉（color、background、border）
  5. 动画（transition、animation）
- 缩进 2 空格

## JavaScript 规范

- 使用 ES6+ 语法：
  - `const` / `let` 替代 `var`
  - 箭头函数
  - 模板字符串
  - 解构赋值
  - 展开运算符
  - 可选链 `?.` 和空值合并 `??`
- DOM 操作：
  - 使用 `querySelector` / `querySelectorAll`，禁止 `getElementById` 混用
  - 批量 DOM 操作使用 `DocumentFragment`
  - 事件委托优先，减少事件监听器数量
  - 使用 `addEventListener`，禁止 `onclick` 内联
- 异步编程：
  - 统一使用 `async/await`
  - Promise 链必须包含 `.catch()`
  - 并发请求使用 `Promise.all()` / `Promise.allSettled()`
- 模块化：
  - 使用 ES Modules（`import` / `export`）
  - 禁止全局变量污染
  - 每个模块单一职责
- 缩进 2 空格

## 注释规范

- 所有注释使用中文
- 文件顶部必须加注释说明文件用途
- 函数必须有中文 JSDoc 注释：`@param`、`@returns`、`@throws`
- 复杂逻辑必须添加行内注释说明意图
- CSS 关键布局和 hack 必须注释说明
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 代码质量强制要求

- 禁止空指针：所有可能为 null/undefined 的值必须判空，禁止信任外部输入
- 禁止魔法值：硬编码常量必须定义为命名常量
- 禁止使用 `==` 比较，统一使用 `===` 严格相等
- 集合操作前必须判空，使用 `Array.isArray()` + `.length`
- DOM 操作必须确保元素存在后再操作
- 异步操作必须处理错误状态
- 禁止使用 `eval()`、`with`、`document.write()`
- 禁止在内联事件处理器中编写 JS（`onclick="..."`）
- 冗余代码、`console.log` 上线前必须清理

## 性能优化

- CSS 放在 `<head>`，JS 放在 `</body>` 前或使用 `defer`
- 图片使用 WebP 格式 + 懒加载（`loading="lazy"`）
- 使用 `requestAnimationFrame` 处理动画
- 防抖（debounce）和节流（throttle）处理高频事件
- 长列表使用虚拟滚动
- 资源预加载：`<link rel="preload">`

## 安全规范

- 用户输入必须转义后再插入 DOM，防止 XSS
- 使用 `textContent` 替代 `innerHTML`（不需要 HTML 解析时）
- Cookie 设置 `HttpOnly`、`Secure`、`SameSite`
- 外部链接添加 `rel="noopener noreferrer"`
- 禁止在客户端存储敏感信息

## 最佳实践

- 使用 CSS 自定义属性实现主题切换
- 使用 `IntersectionObserver` 实现懒加载和滚动监听
- 使用 `ResizeObserver` 监听元素尺寸变化
- 使用 `MutationObserver` 监听 DOM 变化
- 使用 Web Storage（localStorage/sessionStorage）缓存数据
- 使用 `fetch` API 进行网络请求
- 使用 Service Worker 实现离线缓存
