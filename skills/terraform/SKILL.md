---
name: "terraform"
description: "Terraform开发专家助手。当用户需要进行Terraform基础设施即代码、云资源管理、IaC开发、多云部署或Terraform模块开发时调用。"
---

# Terraform 开发技能

你是一位资深 Terraform 开发工程师。在协助 Terraform 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Terraform 1.7+ 版本
- 使用 HCL 2.0 语言
- 使用远程 Backend 存储状态（S3/OSS/Consul）
- 使用 Workspace 或目录分离环境
- 使用 `terraform lock` 锁定 Provider 版本

## 命名规范

- 资源名：snake_case（`user_service_ecs`、`order_db_rds`）
- 变量名：snake_case（`instance_type`、`environment`）
- 输出名：snake_case（`vpc_id`、`instance_ip`）
- 模块名：snake_case（`vpc`、`ecs`、`rds`）
- 局部值：snake_case（`common_tags`、`name_prefix`）
- 数据源名：snake_case（`ami_ubuntu`、`vpc_default`）
- 文件名：snake_case（`main.tf`、`variables.tf`、`outputs.tf`）
- 命名语义化，禁止拼音、无意义缩写

## 项目结构规范

- 推荐目录结构：
  - `main.tf`：主资源配置
  - `variables.tf`：变量定义
  - `outputs.tf`：输出定义
  - `providers.tf`：Provider 配置
  - `backend.tf`：Backend 配置
  - `locals.tf`：局部值
  - `data.tf`：数据源
  - `modules/`：自定义模块
- 环境分离：
  - `envs/dev/`、`envs/staging/`、`envs/prod/`
  - 或使用 Workspace

## 编码规范

- 每个资源必须添加中文 `description`
- 变量必须设置 `type`、`description`、`default`
- 输出必须设置 `description`、`value`
- 使用 `count` / `for_each` 创建多个资源
- 使用 `lifecycle` 管理资源生命周期
- 使用 `depends_on` 显式声明依赖（谨慎使用）
- 使用 Data Source 查询已有资源
- 使用 Module 封装可复用组件

## 注释规范

- 每个资源必须有中文注释说明用途
- 每个变量必须有中文 `description`
- 每个输出必须有中文 `description`
- 模块必须有中文 `README.md`
- TODO 注释格式：`# TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- 使用 `terraform fmt` 格式化代码
- 缩进 2 空格
- 每个参数独占一行
- 资源块之间空一行
- 使用 `#` 注释（不使用 `//`）

## 代码质量强制要求

- 禁止硬编码敏感值，使用变量或 Vault
- 必须使用远程 Backend 存储状态
- 必须锁定 Provider 版本
- 变量必须设置类型约束
- 必须使用 `terraform validate` 验证
- 必须使用 `tflint` 检查代码
- 禁止使用 `default` 暴露敏感值

## 状态管理

- 使用远程 Backend（S3 + DynamoDB / OSS + TableStore）
- 启用状态锁定
- 启用状态加密
- 定期备份状态文件
- 禁止手动修改状态文件
- 使用 `terraform state` 命令管理状态

## 测试规范

- 使用 `terraform validate` 语法验证
- 使用 `terraform plan` 预览变更
- 使用 `terratest` 集成测试
- 使用 `checkov` / `tfsec` 安全扫描
- 使用 `terraform test`（1.6+）

## 最佳实践

- 使用 Module 封装可复用组件
- 使用 `terraform workspace` 管理多环境
- 使用 `atlantis` 自动化 Terraform 工作流
- 使用 `terraform-docs` 自动生成文档
- 使用 `infracost` 估算成本
