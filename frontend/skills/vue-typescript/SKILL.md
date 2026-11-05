---
name: "vue-typescript"
description: "Vue + TypeScript 增量技能。当 Vue 项目需要强制使用 TypeScript、配置 tsconfig 或类型规范时调用,须与 vue 技能配合使用。"
---

# Vue + TypeScript 开发技能

> 本技能为增量技能，须先加载 `vue` 技能，本技能仅补充 TypeScript 相关约束。

## TypeScript 强制约束

- 必须使用 TypeScript，禁止 `any` 滥用，能定义接口/类型就不使用隐式类型
- 枚举统一使用 TS `enum` 定义
- `<script setup>` 内部顺序在 vue 技能基础上增加：类型/接口定义（置于导入依赖之后）
- 使用 `defineProps` 和 `defineEmits` 配合 TypeScript 接口定义组件接口

## 类型定义规范

- 接口、类型统一抽入 `types` 目录，业务实体定义 `interface`
- 请求入参、返回值必须定义 TS 类型，禁止随意 `any`
- 函数必须标注入参类型和返回值类型
- 复用类型全局定义，避免重复声明

## tsconfig 要点

- 优先继承 `@vue/tsconfig` 官方预设，开启 `strict` 严格模式
- 使用 `vue-tsc` 执行类型检查，确保构建前类型无误
