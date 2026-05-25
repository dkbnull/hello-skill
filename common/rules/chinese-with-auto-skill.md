---
name: "chinese-with-auto-skill"
description: "强制使用中文回答，并根据任务上下文自动加载对应的SKILL。当用户发起任何请求时触发此规则。"
---

# 中文回答与自动加载SKILL规则

## 规则一：使用中文回答

- **所有回复必须使用中文**，包括解释、建议、错误分析等
- 代码注释使用中文（除非用户明确要求使用其他语言）
- 变量名、函数名等代码标识符遵循项目既有规范，不受此规则约束
- 代码中的字符串内容根据业务需要决定语言，不受此规则约束

## 规则二：自动加载对应SKILL

在回复用户请求之前，必须根据任务上下文**自动识别并加载**最相关的SKILL。识别逻辑如下：

### 识别原则

1. **优先匹配**：根据用户请求的技术领域、框架、语言，匹配对应的SKILL
2. **多SKILL组合**：一个任务可能涉及多个SKILL，按需组合加载
3. **通用SKILL兜底**：当无法精确匹配时，加载通用类SKILL（如 code-generation、code-review、debug 等）

### 匹配映射

| 任务类型 | 可能的SKILL |
|---------|------------|
| 编写新功能/生成代码 | code-generation, requirement-analysis |
| 代码审查/质量检查 | code-review, defensive-programming |
| Bug修复/排错 | debug, error-handling |
| 性能优化 | performance, refactoring |
| 代码重构 | refactoring, design-patterns |
| API设计 | api-design, role-switch(架构师) |
| 技术文档编写 | tech-doc |
| 需求分析 | requirement-analysis, structured-thinking |
| 前端开发 | 对应框架、对应编程语言的SKILL |
| 后端开发 | 对应框架、对应编程语言的SKILL |
| 数据库操作 | 对应数据库的SKILL |
| DevOps/部署 | 对应工具的SKILL |
| 移动端/桌面端 | 对应平台的SKILL |
| 特定编程语言 | 对应语言的SKILL |

### 执行流程

1. **分析请求**：理解用户当前任务的技术领域和意图
2. **匹配SKILL**：根据上述映射表找到最相关的SKILL
3. **加载SKILL**：调用 Skill 工具加载对应的SKILL，获取专业指导
4. **遵循SKILL规范**：在SKILL指导下完成任务
5. **角色声明**：若涉及 role-switch，在回复开头以 `【当前角色：XXX】` 格式声明

### 示例

- 用户说"帮我写一个React登录页面" → 加载 `react` SKILL + `code-generation` SKILL
- 用户说"这段Java代码有Bug" → 加载 `debug` SKILL + `java` SKILL
- 用户说"帮我设计一个RESTful API" → 加载 `api-design` SKILL
- 用户说"优化这个SQL查询" → 加载 `performance` SKILL + `mysql`(或其他数据库) SKILL
- 用户说"用Spring Boot写一个用户管理接口" → 加载 `springboot` SKILL + `java` SKILL + `code-generation` SKILL
- 用户说"这个Spring Boot项目的接口响应很慢" → 加载 `springboot` SKILL + `performance` SKILL
- 用户说"帮我重构这个Java项目" → 加载 `java` SKILL + `refactoring` SKILL + `design-patterns` SKILL
