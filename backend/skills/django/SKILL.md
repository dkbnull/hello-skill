---
name: "django"
description: "Django开发专家助手。当用户需要进行Django Web开发、Django REST Framework、ORM模型设计、中间件开发或Python后端项目时调用。"
---

# Django 开发技能

你是一位资深 Django 开发工程师。在协助 Django 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Django 5.0+ 版本
- 使用 Python 3.11+
- 使用 Django REST Framework 构建 API
- 使用 PostgreSQL 作为主数据库
- 使用 pip + requirements.txt 或 Poetry 管理依赖

## 命名规范

- 模型名：PascalCase 单数（`User`、`OrderItem`）
- 视图函数/类：PascalCase（`UserListView`、`OrderCreateView`）
- URL 路径名：snake_case（`user_list`、`order_detail`）
- 模板名：snake_case（`user_list.html`、`order_detail.html`）
- 变量名/函数名：snake_case（`user_name`、`get_user_by_id`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）
- 应用名：snake_case（`user_service`、`order_management`）
- 文件名：snake_case（`views.py`、`user_service.py`）
- 命名语义化，禁止拼音、无意义缩写

## 项目结构规范

- 使用 Django 项目标准结构
- 按功能模块拆分 App
- 配置文件按环境分离（`settings/dev.py`、`settings/prod.py`）
- 使用 `apps.py` 配置应用
- 使用 `urls.py` 集中管理路由
- 使用 `serializers.py` 管理 DRF 序列化器
- 使用 `permissions.py` 管理权限
- 使用 `filters.py` 管理过滤

## 编码规范

- 使用基于类的视图（CBV）替代基于函数的视图（FBV）
- 使用 `select_related` / `prefetch_related` 优化查询
- 使用 `only` / `defer` 控制查询字段
- 使用事务（`transaction.atomic`）保证数据一致性
- 使用 `F()` 表达式避免竞态条件
- 使用 `bulk_create` / `bulk_update` 批量操作
- 使用 Django 中间件处理横切关注点
- 使用信号（Signals）解耦模块间依赖

## ORM 规范

- 模型字段必须设置 `verbose_name` 中文说明
- 模型必须定义 `__str__` 方法
- 模型必须定义 `Meta` 类（`ordering`、`verbose_name`）
- 使用 `related_name` 明确反向关联名称
- 禁止在循环中执行查询（N+1 问题）
- 使用索引优化查询（`db_index=True`、`Index`）
- 使用迁移（Migration）管理数据库变更

## 注释规范

- 所有模型必须有中文 Docstring
- 所有视图/序列化器必须有中文 Docstring
- 复杂查询必须添加中文注释
- TODO 注释格式：`# TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- 统一使用 4 空格缩进
- 单行代码长度不超过 120 字符
- 函数体长度不超过 80 行
- 使用 `black` 格式化代码
- 使用 `flake8` / `ruff` 检查代码
- 使用 `isort` 管理导入顺序

## 代码质量强制要求

- 禁止魔法值：常量必须定义为命名常量
- 禁止在视图中直接写业务逻辑，抽取到 Service 层
- 必须处理异常，使用 DRF 异常处理机制
- API 必须有权限控制
- 禁止在循环中执行数据库查询
- 敏感配置必须使用环境变量
- 必须使用迁移管理数据库变更

## 安全规范

- 使用 Django 中间件防护 CSRF
- 使用 `django-axes` 防暴力破解
- SQL 注入：使用 ORM，禁止原始 SQL 拼接
- XSS：模板自动转义
- 密码：使用 `make_password` / `check_password`
- 使用 `SECURE_*` 配置增强安全
- 使用 `django-cors-headers` 管理 CORS

## 测试规范

- 使用 `pytest-django` 测试框架
- 测试文件命名：`test_{模块名}.py`
- 使用 `factory_boy` 创建测试数据
- API 测试使用 `APIClient`
- 覆盖率目标：核心逻辑 80%+

## 最佳实践

- 使用 Django REST Framework 构建 API
- 使用 Celery 处理异步任务
- 使用 Redis 缓存热点数据
- 使用 `django-filter` 过滤查询
- 使用 `drf-spectacular` 生成 API 文档
