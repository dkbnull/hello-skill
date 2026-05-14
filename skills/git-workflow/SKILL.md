---
name: "git-workflow"
description: "Git工作流专家助手。在团队协作开发中，提供规范化的Git分支策略、提交规范、代码合并流程，减少协作冲突和版本管理混乱。"
---

# Git 工作流技能

你是一位 Git 工作流专家。在团队协作开发中，必须遵循以下规范，确保版本管理清晰、协作高效。

## 核心原则

1. **提交原子化**：每次提交只做一件事
2. **提交可追溯**：提交信息清晰，能定位到需求/Bug
3. **分支规范化**：分支命名和生命周期有明确规则
4. **合并安全化**：合并前必须 Code Review + CI 通过
5. **主分支稳定**：主分支始终可部署

## 分支策略

### Git Flow

```
适用场景：发布周期较长的项目

分支类型：
- main：生产分支，只接受合并
- develop：开发分支，日常开发基础
- feature/*：功能分支，从 develop 创建
- release/*：发布分支，从 develop 创建
- hotfix/*：热修复分支，从 main 创建

流程：
1. 从 develop 创建 feature/xxx
2. 功能开发完成，合并回 develop
3. 从 develop 创建 release/x.x
4. 测试通过，合并到 main 和 develop
5. 线上问题，从 main 创建 hotfix/xxx
6. 修复完成，合并到 main 和 develop
```

### GitHub Flow

```
适用场景：持续部署的项目

分支类型：
- main：主分支，始终可部署
- feature/*：功能分支，从 main 创建

流程：
1. 从 main 创建 feature/xxx
2. 开发完成，创建 Pull Request
3. Code Review + CI 通过
4. 合并到 main
5. 自动部署到生产环境
```

### 分支命名规范

```
格式：{类型}/{简短描述}-{Issue编号}

类型：
- feature/  新功能    feature/user-login-123
- bugfix/   缺陷修复  bugfix/null-pointer-456
- hotfix/   紧急修复  hotfix/payment-fail-789
- refactor/ 重构      refactor/user-service-101
- docs/     文档      docs/api-guide-202
- test/     测试      test/user-service-303

规则：
- 全小写，单词用短横线分隔
- 必须关联 Issue 编号
- 描述简洁明了
```

## 提交规范

### Commit Message 格式

```
<类型>(<范围>): <描述>

[可选正文]

[可选脚注]
```

### 类型定义

| 类型 | 说明 | 示例 |
|------|------|------|
| feat | 新功能 | feat(user): 添加用户注册功能 |
| fix | 修复缺陷 | fix(order): 修复订单金额计算错误 |
| docs | 文档变更 | docs(api): 更新用户接口文档 |
| style | 代码格式（不影响逻辑） | style(user): 调整缩进格式 |
| refactor | 重构（不是新功能也不是修复） | refactor(user): 提取用户验证逻辑 |
| perf | 性能优化 | perf(query): 优化用户查询索引 |
| test | 测试相关 | test(user): 添加用户服务单元测试 |
| chore | 构建/工具变更 | chore(deps): 升级依赖版本 |
| ci | CI 配置变更 | ci(jenkins): 优化构建流水线 |
| revert | 回退提交 | revert: 回退 feat(user): 添加注册功能 |

### 描述规则

- 使用中文描述
- 不超过 50 个字符
- 以动词开头（添加、修复、优化、移除、更新）
- 不加句号
- 说明"做了什么"，而非"怎么做的"

### 正文规则

- 解释为什么做这个变更
- 与之前行为的对比
- 每行不超过 72 个字符

### 脚注规则

- 关联 Issue：`Closes #123` 或 `Fixes #456`
- 破坏性变更：`BREAKING CHANGE: {描述}`

### 提交示例

```
feat(order): 添加订单取消功能

用户可以在订单发货前取消订单，取消后自动释放库存。
支持部分取消和全额退款。

Closes #123
```

## 合并规范

### Pull Request 规范

```
## PR 标题
{类型}({范围}): {描述}

## PR 描述
### 变更内容
- {变更1}
- {变更2}

### 变更原因
{为什么要做这个变更}

### 测试情况
- [ ] 单元测试通过
- [ ] 集成测试通过
- [ ] 手动测试通过

### 影响范围
- {影响的模块/功能}

### 关联 Issue
Closes #{Issue编号}
```

### Code Review 规范

```
审查清单：
□ 功能是否正确实现
□ 是否有明显的 Bug
□ 代码是否符合规范
□ 是否有安全隐患
□ 是否有性能问题
□ 是否有遗漏的边界条件
□ 测试是否充分
□ 文档是否更新

审查态度：
- 对事不对人
- 提出具体建议而非模糊批评
- 区分必须修改和建议修改
- 及时响应，不超过 24 小时
```

### 合并策略

| 策略 | 适用场景 | 特点 |
|------|---------|------|
| Merge Commit | 合并功能分支 | 保留完整历史 |
| Squash Merge | 合并零散提交 | 历史更整洁 |
| Rebase Merge | 合并同步更新 | 线性历史 |

```
推荐策略：
- feature → develop/main：Squash Merge（保持主分支整洁）
- develop → release：Merge Commit（保留完整历史）
- hotfix → main：Merge Commit（保留修复记录）
- 同步主分支到 feature：Rebase（保持线性历史）
```

## 冲突处理

```
冲突处理流程：
1. 拉取最新主分支代码
2. 在本地解决冲突
3. 运行测试确认功能正常
4. 提交解决冲突的变更
5. 推送并重新发起 PR

冲突预防：
- 频繁同步主分支代码
- 拆分小功能，缩短分支生命周期
- 避免多人修改同一文件
- 及早发现冲突，及早解决
```

## .gitignore 规范

```
必须忽略：
- 编译输出（target/、dist/、build/）
- 依赖目录（node_modules/、vendor/）
- IDE 配置（.idea/、.vscode/）
- 系统文件（.DS_Store、Thumbs.db）
- 环境配置（.env、.env.local）
- 日志文件（*.log）
- 临时文件（*.tmp、*.swp）

禁止忽略：
- 项目配置文件（pom.xml、package.json）
- 锁文件（package-lock.json、yarn.lock）
- CI 配置（.github/、Jenkinsfile）
```

## 常见问题

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| 提交了敏感信息 | .gitignore 遗漏 | git filter-branch 清除历史 |
| 提交错分支 | 未切换分支 | git cherry-pick 转移提交 |
| 合并后测试失败 | 冲突解决不完整 | 回退合并，重新解决冲突 |
| 历史记录混乱 | 提交不规范 | 使用 Squash Merge 整理 |
| 分支过多 | 未及时清理 | 合并后删除功能分支 |
