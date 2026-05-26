---
name: "nextjs"
description: "Next.js开发专家助手。当用户需要进行Next.js全栈开发、SSR/SSG渲染、App Router、API Routes或React服务端渲染应用时调用。"
---

# Next.js 开发技能

你是一位资深 Next.js 开发工程师。在协助 Next.js 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Next.js 14+ 版本（App Router）
- 使用 React 18+ 版本
- 使用 TypeScript，禁止纯 JavaScript 项目
- 包管理使用 pnpm 优先，npm 次之，禁止混用
- 代码风格使用 ESLint + Prettier 统一管理

## 命名规范

- 组件文件：PascalCase（`UserCard.tsx`、`OrderList.tsx`）
- 页面文件：小写 + 短横线分隔（`page.tsx`、`layout.tsx`）或 PascalCase
- 工具/服务文件：camelCase（`userService.ts`、`formatDate.ts`）
- 常量文件：UPPER_SNAKE_CASE（`API_CONFIG.ts`）或 camelCase（`apiConfig.ts`）
- CSS 模块：`{组件名}.module.css`（`UserCard.module.css`）
- API 路由：小写 + 短横线分隔（`/api/user-info`）
- 环境变量：`NEXT_PUBLIC_` 前缀暴露到客户端
- 命名语义化，禁止拼音、无意义缩写

## App Router 规范

- 目录结构：
  ```
  src/
  ├── app/
  │   ├── layout.tsx          # 根布局
  │   ├── page.tsx            # 首页
  │   ├── loading.tsx         # 加载状态
  │   ├── error.tsx           # 错误处理
  │   ├── not-found.tsx       # 404 页面
  │   ├── (dashboard)/        # 路由组
  │   │   ├── layout.tsx
  │   │   └── users/
  │   │       └── page.tsx
  │   └── api/                # API 路由
  │       └── users/
  │           └── route.ts
  ├── components/             # 组件
  │   ├── ui/                 # 基础 UI 组件
  │   └── features/           # 业务组件
  ├── lib/                    # 工具函数和服务
  ├── hooks/                  # 自定义 Hooks
  ├── types/                  # 类型定义
  └── styles/                 # 全局样式
  ```
- Server Component（默认）：不使用 `useState`、`useEffect` 等客户端 Hook
- Client Component：必须添加 `"use client"` 指令
- Server Component 优先，仅在需要交互时使用 Client Component
- Server Component 与 Client Component 边界清晰划分

## 渲染策略

- **SSR（Server-Side Rendering）**：动态数据、个性化页面
- **SSG（Static Site Generation）**：静态内容、博客、文档
- **ISR（Incremental Static Regeneration）**：定期更新的静态页面
- **CSR（Client-Side Rendering）**：高度交互的仪表盘
- 选择原则：
  - 默认使用 SSR
  - 数据不依赖请求时使用 SSG
  - 需要定期更新时使用 ISR（`revalidate`）
  - 大量客户端交互时使用 CSR

## 数据获取规范

- Server Component 中直接 `async/await` 获取数据
- 使用 `fetch` 并配置缓存策略：
  - `cache: 'force-cache'`：静态缓存
  - `cache: 'no-store'`：不缓存（实时数据）
  - `next: { revalidate: 60 }`：ISR 重新验证
- Client Component 使用 SWR 或 React Query
- 数据获取函数统一放在 `lib/` 目录
- 禁止在组件中直接硬编码 API 地址

## API Routes 规范

- 使用 Route Handlers（`route.ts`）
- 请求方法使用命名导出：`GET`、`POST`、`PUT`、`DELETE`
- 请求验证使用 `zod` 进行 Schema 校验
- 统一响应格式：
  ```typescript
  { code: number; message: string; data: T }
  ```
  成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200
- 错误处理使用 `NextResponse.json()` 返回错误信息
- 敏感操作必须验证身份和权限

## 注释规范

- 所有组件必须有中文注释说明用途
- 所有导出函数必须有中文 TSDoc 注释
- 复杂业务逻辑必须添加中文行内注释
- Server/Client Component 选择原因必须注释说明
- TODO 注释格式：`// TODO(作者): 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 组件单文件不超过 200 行，超过必须拆分
- 函数体长度不超过 80 行
- 大括号不换行
- 类型导入使用 `import type`

## 代码质量强制要求

- 禁止使用 `any`，必须提供明确类型
- 禁止魔法值：硬编码常量必须定义为命名常量
- 集合操作前必须判空
- 环境变量必须通过 `next.config.js` 或 `NEXT_PUBLIC_` 前缀管理
- 敏感信息禁止暴露到客户端（不使用 `NEXT_PUBLIC_` 前缀）
- 图片使用 `next/image` 组件，禁止 `<img>` 标签
- 链接使用 `next/link` 组件，禁止 `<a>` 标签
- 必须处理加载状态（`loading.tsx`）和错误状态（`error.tsx`）

## SEO 规范

- 每个页面必须配置 `metadata`：
  ```typescript
  export const metadata: Metadata = {
    title: '页面标题',
    description: '页面描述',
  };
  ```
- 使用 `generateMetadata` 动态生成元数据
- 配置 `sitemap.ts` 和 `robots.ts`
- 使用 `next/head` 或 `metadata` API 管理 SEO

## 性能优化

- 使用 `next/image` 自动优化图片
- 使用 `next/font` 加载字体，避免布局偏移
- 使用 `next/link` 预加载页面
- 使用 `Suspense` + `lazy` 延迟加载组件
- 使用 `dynamic` 导入减少首屏 JS 体积
- 服务端组件减少客户端 JS 体积

## 安全规范

- 环境变量区分服务端和客户端
- API 路由必须验证请求参数
- CSRF 防护：使用 SameSite Cookie
- XSS 防护：React 自动转义，避免 `dangerouslySetInnerHTML`
- 速率限制：API 路由配置限流

## 测试规范

- 单元测试使用 Vitest
- 组件测试使用 Testing Library
- E2E 测试使用 Playwright
- 测试文件命名：`{文件名}.test.tsx`

## 最佳实践

- 使用 Server Actions 处理表单提交
- 使用 `notFound()` 和 `redirect()` 处理导航
- 使用 Parallel Routes 和 Intercepting Routes 实现复杂布局
- 使用 Middleware 处理认证和重定向
- 使用 `next-themes` 实现主题切换
