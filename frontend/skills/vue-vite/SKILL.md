---
name: "vue-vite"
description: "Vue + Vite 增量技能。当 Vue 项目需要配置 Vite(代理、插件、环境变量、构建优化)时调用,须与 vue 技能配合使用。"
---

# Vue + Vite 开发技能

> 本技能为增量技能，须先加载 `vue` 技能，本技能仅补充 Vite 工程化相关约束。

> Vite 通用配置（别名、代理、环境变量、分包）遵循 vite 技能。

## 插件

- 开发环境使用 `vite-plugin-vue-devtools`
- 使用 `unplugin-auto-import` 和 `unplugin-vue-components` 实现自动导入

## API 层（项目脚手架约定，与构建无关）

- 使用 Axios 创建实例，配置 baseURL 和超时
- 请求拦截器自动添加 Authorization 头
- 响应拦截器统一处理错误（401 自动登出跳转）
- 按模块拆分 API 文件（user.js、auth.js），组件只调用函数不直接写请求地址
- 接口返回数据统一封装固定结构，前端统一处理成功/失败回调
