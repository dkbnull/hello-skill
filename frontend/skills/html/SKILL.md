---
name: "html"
description: "HTML开发专家助手。当用户需要进行HTML页面结构开发、语义化标签、表单或Web文档结构设计时调用。样式遵循 css 技能，交互遵循 javascript 技能。"
---

# HTML 开发技能

你是一位资深 Web 前端开发工程师。在协助 HTML 开发时，请遵循以下规范。

## 技术栈强制约束

- 使用 HTML5 语义化标签
- 禁止使用已废弃标签（`<font>`、`<center>`、`<marquee>` 等）

## 命名规范

- HTML 文件：小写 + 短横线分隔（`user-list.html`、`order-detail.html`）
- ID 命名：小写 + 短横线分隔（`user-list`、`order-form`）
- 命名语义化，禁止拼音、无意义缩写

## 结构规范

- 使用语义化标签：`<header>`、`<nav>`、`<main>`、`<article>`、`<section>`、`<aside>`、`<footer>`
- 禁止滥用 `<div>` 和 `<span>`
- 表单元素必须关联 `<label>`
- 图片必须添加 `alt` 属性
- 链接必须添加有意义的链接文本
- 属性顺序：`id` → `class` → `data-*` → `src/href` → `title/alt`
- 自闭合标签不加斜杠：`<br>`、`<img>`、`<input>`
- 缩进 2 空格

## 注释规范

- 所有注释使用中文
- 文件顶部必须加注释说明文件用途
- 禁止无意义注释

## 性能与安全

- 图片使用 WebP 格式 + 懒加载（`loading="lazy"`）
- 资源预加载：`<link rel="preload">`
- CSS 放在 `<head>`，JS 放在 `</body>` 前或使用 `defer`
- 外部链接添加 `rel="noopener noreferrer"`
- 禁止内联事件处理器（`onclick="..."`），交互逻辑交由 JS 通过 `addEventListener` 绑定

## 最佳实践

- 使用 `IntersectionObserver` 实现懒加载和滚动监听
- 使用 `MutationObserver` 监听 DOM 变化
