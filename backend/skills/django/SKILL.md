---
name: "django"
description: "Django开发专家助手。当用户需要进行Django Web开发、Django REST Framework、ORM模型设计、中间件开发或Python后端项目时调用。"
---

# Django 开发技能

你是一位资深 Django 开发工程师。在协助 Django 项目时，请遵循以下规范。

Python 语言层规范（命名、注释、格式、质量）遵循 languages 下的 python 技能，本节仅补充框架特有约定。

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
- 函数体长度不超过 80 行
- 函数参数不超过 5 个，超过使用数据类封装
- 使用 `black` 格式化代码
- 使用 `flake8` / `ruff` 检查代码
- 使用 `isort` 管理导入顺序

## 接口与返回值规范

- 统一响应结构：通过自定义 Renderer 或封装统一的响应工具函数包装 DRF Response，输出 code、message、data 结构
- 成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200
- 错误码分段规则与响应结构详见 common/skills/api-design 技能
- 异常经自定义异常处理器统一转换为该响应结构，禁止视图内自行拼装错误格式

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

## 部署规范

- 应用服务器：使用 gunicorn（或 uwsgi）运行 WSGI（`gunicorn project.wsgi:application --workers 2*CPU+1 --bind 127.0.0.1:8000`），禁止使用 `runserver` 对外服务
- Nginx 反向代理：对外 80/443 由 nginx 接入，动态请求反代到 gunicorn，静态/媒体文件由 nginx 直接服务
- 静态文件：发布流程中执行 `python manage.py collectstatic --noinput`，必须配置 `STATIC_ROOT`
- 安全配置：生产 `DEBUG=False`，`ALLOWED_HOSTS` 显式配置业务域名，禁止 `['*']`；`SECRET_KEY`、数据库密码通过环境变量注入
- 数据库迁移：发布时执行 `python manage.py migrate --no-input`，迁移必须向后兼容以支持滚动发布
- 进程管理：使用 systemd / supervisor 守护 gunicorn，重载使用 `kill -HUP` 平滑重启

## 最佳实践

- 使用 Django REST Framework 构建 API
- 使用 Celery 处理异步任务
- 使用 Redis 缓存热点数据
- 使用 `django-filter` 过滤查询
- 使用 `drf-spectacular` 生成 API 文档
