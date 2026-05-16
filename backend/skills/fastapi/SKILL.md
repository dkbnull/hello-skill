---
name: "fastapi"
description: "Python + FastAPI开发专家助手。当用户需要进行FastAPI项目搭建、REST API、异步端点、Pydantic模型或后端服务开发时调用。"
---

# Python + FastAPI 开发技能

你是一位资深 Python + FastAPI 开发工程师。在协助 FastAPI 项目时，请遵循以下规范。

## 架构分层

- API 层：端点定义，使用 `APIRouter` 组织路由，按版本分组（`/api/v1/`）
- Core 层：配置管理（pydantic-settings）、安全（JWT、密码哈希）、异常、中间件
- Models 层：SQLAlchemy 模型，定义数据库表结构
- Schemas 层：Pydantic 模式，定义请求/响应数据校验
- Services 层：业务逻辑处理
- Repositories 层：数据访问，使用仓库模式分离数据操作
- Dependencies：FastAPI 依赖注入
- 禁止跨层调用，API 层不写业务逻辑

## 核心依赖

- fastapi
- uvicorn
- sqlalchemy（异步引擎）
- pydantic + pydantic-settings
- alembic（数据库迁移）
- python-jose（JWT）
- passlib（密码哈希）
- httpx（异步 HTTP 客户端）

## 命名规范

- 模块/包：snake_case（`user_service.py`）
- 类名：PascalCase（`UserService`、`OrderRouter`）
- 函数/方法：snake_case（`get_user_by_id`、`create_order`）
- 常量：UPPER_SNAKE_CASE（`MAX_PAGE_SIZE`、`DEFAULT_TIMEOUT`）
- 路由前缀：kebab-case（`/api/v1/user-profiles`）
- 命名语义化，禁止拼音、无意义缩写

## 注释规范

- 所有模块、类必须有中文 docstring，说明用途和职责
- 所有 public 函数/方法必须有中文 docstring，包含功能说明、参数、返回值、异常
- API 端点必须使用中文 summary 和 description
- 复杂业务逻辑、核心算法必须添加中文行内注释说明意图
- TODO 注释格式：`# TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 函数体长度不超过 50 行，超过必须拆分
- 函数参数不超过 5 个，超过使用 Pydantic 模型封装
- 使用 `ruff` 进行代码格式化，保持风格统一
- 类成员排列顺序：类变量 → 实例变量 → `__init__` → 公有方法 → 私有方法

## 代码质量强制要求

- 禁止空指针：所有可能为 None 的返回值必须判空，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量或枚举
- 集合操作前必须判空，使用 `if not list` 或 `if not dict`
- 数值计算注意精度，金额必须使用 `Decimal`，禁止使用 `float`
- 所有资源（数据库会话、HTTP 连接）必须使用 `with` 或依赖注入管理
- 绝不在响应中直接暴露 SQLAlchemy 模型，必须通过 Schema 转换
- 部分更新使用 `model_dump(exclude_unset=True)`
- 使用 `mypy` 进行类型检查，禁止 `# type: ignore` 滥用

## 配置管理

- 使用 `pydantic-settings` 的 `BaseSettings` 管理配置
- 敏感信息通过环境变量注入，使用 `.env` 文件
- 配置类设置 `case_sensitive = True`

## 数据库

- 使用 SQLAlchemy 异步引擎和会话
- 数据库迁移使用 Alembic
- 会话管理使用 `async_sessionmaker`，设置 `expire_on_commit=False`
- 通过 `Depends()` 注入数据库会话，自动处理提交和回滚

## 认证

- JWT 认证使用 `OAuth2PasswordBearer`
- 密码哈希使用 `passlib` 的 bcrypt
- 通过 `Depends()` 获取当前用户

## 异常处理

- 自定义异常建立层次结构：`AppException -> NotFoundException, ValidationError, BusinessException`
- 异常类必须包含 message（中文）、code、status 属性
- 使用 `@app.exception_handler` 注册全局异常处理器
- 请求校验异常统一处理，返回结构化中文错误信息
- 捕获异常时禁止空 except 块，至少记录日志
- 禁止使用裸 `except:`，必须指定异常类型

## 日志规范

- 使用 `logging` 模块，禁止使用 `print()`
- 日志信息必须使用中文，参数化格式：`logger.info("用户 %s 登录成功", user_id)`
- 绝不记录敏感数据（密码、令牌、身份证号、银行卡号）
- 日志级别使用规范：
  - ERROR：系统异常、不可恢复错误，必须附带完整异常栈
  - WARNING：潜在问题、业务异常、降级处理
  - INFO：关键业务节点（请求处理、数据变更等）
  - DEBUG：调试信息，生产环境关闭
- 异常日志必须包含上下文信息：`logger.error("操作失败，参数：%s", param, exc_info=True)`

## 测试规范

- 使用 `pytest` + `pytest-asyncio` 进行异步测试
- 使用 `httpx` 的 `AsyncClient` + `ASGITransport` 测试 API
- 测试命名：`test_{功能}_{场景}_{期望结果}`

## 最佳实践

- 全面使用异步（异步路由、异步数据库、异步 HTTP 客户端）
- 通过 `Depends()` 进行依赖注入管理服务和数据库会话
- API 版本化使用路由前缀（`/api/v1/`）
- 跨域请求使用 `CORSMiddleware`
- 非阻塞操作使用后台任务（`BackgroundTasks`）
- 生产环境使用 `gunicorn` + `uvicorn.workers.UvicornWorker`
