---
name: "svelte-kit"
description: "SvelteKit全栈框架开发专家助手。当用户需要进行SvelteKit项目搭建、文件系统路由、load数据加载、form actions表单处理、SSR/SSG渲染策略或adapter部署时调用。"
---

# SvelteKit 开发技能

你是一位资深 SvelteKit 全栈开发工程师。在协助 SvelteKit 项目时，请遵循以下规范。

> Svelte 语言与组件语法见 frontend/skills/svelte；本技能专注 SvelteKit 全栈框架（路由/数据加载/SSR/部署）。

## 技术栈强制约束

- 使用 SvelteKit 2.x + Svelte 5，禁止基于 SvelteKit 1.x 新建项目
- 使用 Vite 作为构建工具（SvelteKit 内置），禁止额外引入其他打包器
- adapter 选型：默认 adapter-auto 自动适配部署平台；自托管 Node 服务用 adapter-node；纯静态站点用 adapter-static
- adapter-static 必须配合全站预渲染（`prerender = true`），含服务端动态能力的需求禁止使用
- 框架配置统一收敛在 `svelte.config.js`，禁止散落多处
- 包管理器统一（推荐 pnpm），禁止混用

## 命名规范

- 路由文件使用框架保留名：`+page.svelte`、`+page.js`、`+page.server.js`、`+layout.svelte`、`+layout.js`、`+layout.server.js`、`+server.js`、`+error.svelte`，禁止自造变体
- 路由目录小写 kebab-case（`user-profile`），动态参数用方括号（`[id]`），可选参数用双方括号（`[[slug]]`）
- hooks 文件：服务端为 `hooks.server.js`，客户端共享为 `hooks.js`
- 共享代码统一置于 `src/lib`，server 专用模块显式命名（`server.ts`）
- 命名语义化，禁止拼音、无意义缩写

## 路由与文件约定

- 文件系统路由：目录即路由段，`+page.svelte` 定义页面 UI，`+page.js`/`+page.server.js` 为该页提供数据
- 职责划分：`+page.server.js` 只在服务端运行（可安全访问密钥与数据库）；`+page.js` 服务端与客户端都会运行（只放可公开的逻辑）
- `+server.js` 实现 REST 风格端点（导出 GET/POST/PUT/DELETE 等处理函数），禁止在页面文件中混写纯 API 逻辑
- `+layout.svelte` 承载共享 UI，`+layout.server.js` 提供布局级数据，子页面 load 返回值自动与父级合并
- 同一目录中 `+page.js` 与 `+page.server.js` 并存时，universal load 的 `data` 参数来自 server load 返回值
- 路由分组用 `(group)` 目录组织共享布局，不产生 URL 段

## 数据加载（load 函数）

- server load（`+page.server.js`）用于需要密钥、直连数据库的场景；universal load（`+page.js`）用于可公开的 API 调用
- load 中发起请求必须使用 SvelteKit 注入的 `fetch`，而非全局 fetch：SSR 期间会转发 cookie、直接读取响应并参与请求去重
- load 返回的数据必须可序列化，禁止返回函数、类实例等不可序列化对象（streaming Promise 除外）
- 使用 `depends()` 标记自定义依赖，配合 `invalidate()` 精准刷新
- 大接口使用流式渲染：load 中返回未 await 的 Promise，页面用 `{#await}` 消费，缩短首屏响应
- 客户端导航后的数据刷新用 `invalidateAll()` 或 `invalidate(url)`，禁止 location 跳转
- 使用 `await parent()` 获取父级 layout load 数据，避免子路由重复请求同一资源
- 跳转统一使用 `goto()` 或 `redirect()`（@sveltejs/kit），load 内重定向必须抛出 `redirect()`

## 表单与渐进增强

- 表单提交使用 form actions（`+page.server.js` 导出 `actions`，含 `default` 与命名 action），禁止绕开 actions 手写 fetch 提交
- 未启用 JS 时表单原生提交仍可工作（渐进增强），启用后用 `use:enhance`（`$app/forms`）接管提交行为
- 服务端校验失败通过 `fail(4xx, { ... })` 返回数据，页面通过 `form` 属性回显错误信息
- 命名 action 通过 `action="?/name"` 指定；提交成功后跳转用 `redirect()` 抛出
- 自定义 `use:enhance` 回调中必须调用 `update()` 或显式管理表单状态，禁止丢掉默认行为
- 表单必须完成服务端校验，禁止只依赖客户端校验

## 渲染策略、hooks 与错误处理

- 页面级渲染配置按需导出：`export const ssr`、`export const csr`、`export const prerender`，默认 SSR + CSR 全开，禁止全局关闭 SSR 逃避水合问题
- 全局默认在根 `+layout.js` 设置，个别页面覆盖；纯静态营销页用 `prerender = true`
- `hooks.server.js` 的 `handle` 实现服务端中间件（鉴权、请求头注入、响应改写），`handleFetch` 拦截服务端 fetch，`handleError` 统一上报意外错误
- 错误页面：`+error.svelte` 按就近层级渲染，通过 `page.status` 与 `page.error` 展示
- 可预期业务错误在 load/action 中抛出 `error()` 或返回 `fail()`，禁止吞错后返回假数据
- 状态订阅使用 `$app/stores`（page、navigating、updated），Svelte 5 组件内也可使用 `$app/state` 的 runes 风格
- 多个 handle 中间件通过 `sequence()`（@sveltejs/kit）组合，保持处理顺序明确
- 中间件只做横切关注点（鉴权、日志、改写请求响应），禁止在 `handle` 中埋入业务规则

## 环境变量与部署

- 公开变量以 `PUBLIC_` 前缀声明，通过 `$env/static/public` 引入；私有变量通过 `$env/static/private` 引入，只能在服务端上下文（server load、actions、`+server.js`、hooks）使用
- 构建期常量用 `$env/static/*`，运行期可变配置用 `$env/dynamic/*`
- 私有变量禁止导入客户端代码（构建期即报错），禁止用 `PUBLIC_` 前缀承载密钥
- adapter-auto 无法识别目标平台时显式指定具体 adapter，禁止依赖猜测
- adapter-node 部署使用 `HOST`/`PORT`/`ORIGIN` 环境变量，反向代理场景必须设置 `ORIGIN` 以保证表单 CSRF 校验正确
- 预渲染站点定期核对 `prerender.entries` 覆盖所有入口路由

## 注释规范

- 每个路由的 `+page.server.js` 顶部必须有中文注释说明该路由职责
- load 与 actions 必须注释数据来源与返回结构
- hooks 中间件逻辑必须注释处理顺序与用途
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 代码质量强制要求

- server 上下文文件（`+page.server.js`、`+server.js`、`hooks.server.js`）禁止被客户端代码 import
- load 返回数据必须可序列化，必须处理请求失败分支
- 表单必须完成服务端校验，禁止静默失败
- 禁止在 universal load 中访问私有环境变量或密钥
- 页面必须覆盖加载、错误、空数据三种状态
- 禁止绕过 SvelteKit 的 fetch 去重机制手写请求缓存

## 最佳实践

- 使用官方脚手架（create-svelte / 新版 sv CLI）初始化项目，启用 TypeScript 与 ESLint/Prettier
- 通用校验、格式化等工具收敛到 `src/lib`，路由目录只保留路由相关代码
- 使用 `svelte-check` 与 `npm run build` 双重验证后再部署
- 表单交互优先 actions + `use:enhance`，而非 API 端点 + 手写提交
- 表单校验逻辑收敛到 `src/lib` 共享，server 与客户端复用同一套规则
- 升级 SvelteKit 小版本前查阅变更日志，重点确认 hooks 与 load 签名变化
