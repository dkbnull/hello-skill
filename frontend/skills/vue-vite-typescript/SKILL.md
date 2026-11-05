---
name: "vue-vite-typescript"
description: "Vue + Vite + TypeScript + Tailwind 组合增量技能。当同时使用 Vite、TypeScript 与 Tailwind 的 Vue 项目需要联合配置时调用,须与 vue 技能配合使用。"
---

# Vue + Vite + TypeScript 开发技能

> 本技能为增量技能，须先加载 `vue` 技能，并按需加载 `vue-typescript`、`vue-vite` 增量技能，本技能仅补充三者组合时的专属约束。

## 样式：Tailwind CSS 集成

- 统一使用 Tailwind CSS 编写样式，少写自定义 CSS
- 优先使用 Tailwind 原子类，仅在原子类无法覆盖时编写 `scoped` 自定义样式

## TS + Vite 联合配置要点

> `vite.config.ts` 别名与 `tsconfig` paths 的联合配置遵循 vite 技能。

- 环境变量类型在 `env.d.ts` 中声明，使用 `.d.ts` 文件定义全局接口类型
