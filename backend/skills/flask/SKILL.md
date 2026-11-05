---
name: "flask"
description: "Flask开发专家助手。当用户需要进行Flask轻量级Web开发、REST API、微服务或Python Web应用时调用。"
---

# Flask 开发技能

你是一位资深 Flask 开发工程师。在协助 Flask 项目时，请遵循以下规范。

Python 语言层规范（命名、注释、格式、质量）遵循 languages 下的 python 技能，本节仅补充框架特有约定。

## 技术栈强制约束

- 使用 Flask 3.0+ 版本
- 使用 Python 3.11+
- 使用 Flask-SQLAlchemy 管理 ORM
- 使用 Flask-RESTful 或 Flask-RESTX 构建 API
- 使用 pip + requirements.txt 或 Poetry 管理依赖

## 命名规范

- 蓝图名：snake_case（`user_bp`、`order_bp`）
- 视图函数：snake_case（`get_user_list`、`create_order`）
- 类视图：PascalCase（`UserListView`、`OrderCreateView`）
- 模型名：PascalCase 单数（`User`、`OrderItem`）
- 变量名/函数名：snake_case（`user_name`、`get_user_by_id`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`）
- 文件名：snake_case（`views.py`、`user_service.py`）

## 项目结构规范

- 使用应用工厂模式（`create_app`）
- 使用蓝图（Blueprint）组织模块
- 配置按环境分离（`config/dev.py`、`config/prod.py`）
- 推荐目录结构：
  - `app/`：应用代码
  - `app/models/`：数据模型
  - `app/views/`：视图
  - `app/services/`：业务逻辑
  - `app/utils/`：工具函数
  - `migrations/`：数据库迁移

## 编码规范

- 使用应用工厂模式创建 Flask 实例
- 使用蓝图组织路由
- 使用 Flask-SQLAlchemy 管理 ORM
- 使用 Flask-Migrate 管理数据库迁移
- 使用 `before_request` / `after_request` 处理请求钩子
- 使用 `teardown_appcontext` 清理资源
- 统一响应格式：`{ "code": 0, "message": "操作成功", "data": {} }`，成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200；错误码分段规则与响应结构详见 api-design 技能。
- 统一异常处理

## 注释规范

- 所有模块必须有中文 Docstring
- 所有视图函数必须有中文 Docstring
- 所有模型必须有中文 Docstring
- 复杂逻辑必须添加中文行内注释
- TODO 注释格式：`# TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 格式规范

- 统一使用 4 空格缩进
- 函数体长度不超过 80 行
- 函数参数不超过 5 个，超过使用数据类封装
- 使用 `black` 格式化代码
- 使用 `flake8` / `ruff` 检查代码
- 使用 `isort` 管理导入顺序

## 代码质量强制要求

- 禁止魔法值：常量必须定义为命名常量
- 禁止在视图中直接写业务逻辑，抽取到 Service 层
- 必须使用应用工厂模式
- 必须使用蓝图组织路由
- 敏感配置必须使用环境变量
- 必须处理异常，使用统一错误处理
- 禁止在循环中执行数据库查询

## 安全规范

- 使用 `Flask-WTF` 防护 CSRF
- 使用 `itsdangerous` 签名敏感数据
- 使用 `Flask-Limiter` 限流
- SQL 注入：使用 ORM，禁止原始 SQL 拼接
- 密码：使用 `werkzeug.security`（`generate_password_hash`）
- 使用 `Flask-Talisman` 增强 HTTP 安全头
- 禁止在日志中记录敏感信息

## 测试规范

- 使用 `pytest` + `pytest-flask`
- 测试文件命名：`test_{模块名}.py`
- 使用 `fixture` 准备测试数据
- 使用 `client` 测试 HTTP 接口
- 覆盖率目标：核心逻辑 80%+

## 部署规范

- 应用服务器：生产禁止使用 `flask run` 开发服务器，Linux 使用 gunicorn 运行应用工厂（`gunicorn "app:create_app()" --workers 4 --bind 127.0.0.1:8000`），Windows 使用 waitress（`waitress-serve --call app:create_app`）
- Nginx 反向代理：对外 80/443 由 nginx 接入，动态请求反代到 WSGI 服务器，静态资源与上传文件由 nginx 直接服务
- 环境变量配置：`SECRET_KEY`、数据库连接等敏感配置通过环境变量注入（本地可 `.env` + python-dotenv，`.env` 禁止提交仓库），配置类按环境分离
- 进程管理：使用 systemd / supervisor 守护 WSGI 进程，配置开机自启与异常自动拉起
- 平滑重启：向 gunicorn master 进程发送 HUP 信号，不中断服务加载新代码

## 最佳实践

- 使用 Flask-RESTX 构建 API 并生成 Swagger 文档
- 使用 Celery 处理异步任务
- 使用 Redis 缓存热点数据
- 使用 Flask-JWT-Extended 处理认证
- 使用 Flask-CORS 管理跨域
