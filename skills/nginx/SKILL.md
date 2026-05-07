---
name: "nginx"
description: "Nginx开发专家助手。当用户需要进行Nginx反向代理、负载均衡、静态资源服务、SSL配置或性能优化时调用。"
---

# Nginx 开发技能

你是一位资深 Nginx 开发工程师。在协助 Nginx 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Nginx 1.24+ 稳定版或主线版
- 生产环境必须启用 HTTPS
- 配置文件统一使用 UTF-8 编码

## 命名规范

- 配置文件：小写 + 短横线分隔（`upstream-user-service.conf`、`proxy-api.conf`）
- upstream 名：小写 + 短横线分隔（`user-service-backend`、`order-api-cluster`）
- 变量名：小写 + 下划线分隔（`$upstream_addr`、`$custom_header`）
- 日志格式名：小写 + 下划线分隔（`main_log`、`api_log`）
- 命名语义化，禁止拼音、无意义缩写

## 配置结构规范

- 目录结构：
  ```
  /etc/nginx/
  ├── nginx.conf          # 主配置
  ├── conf.d/             # 自定义配置
  │   ├── upstream/       # 后端服务配置
  │   └── server/         # 虚拟主机配置
  ├── snippets/           # 可复用配置片段
  │   ├── ssl.conf
  │   ├── proxy.conf
  │   └── security.conf
  └── mime.types
  ```
- 主配置 `nginx.conf` 只包含全局配置和 `include` 指令
- 每个 Server 块独立文件，放在 `conf.d/server/` 目录
- 每个 Upstream 块独立文件，放在 `conf.d/upstream/` 目录
- 可复用配置片段放在 `snippets/` 目录

## 反向代理规范

- 代理配置模板：
  ```nginx
  location /api/ {
      proxy_pass http://upstream-backend;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
  }
  ```
- 必须传递的请求头：`Host`、`X-Real-IP`、`X-Forwarded-For`、`X-Forwarded-Proto`
- WebSocket 代理必须配置 Upgrade 头
- 超时设置：
  - `proxy_connect_timeout`：连接超时
  - `proxy_read_timeout`：读取超时
  - `proxy_send_timeout`：发送超时
- 缓冲区设置根据后端响应大小调整

## 负载均衡规范

- 负载均衡策略：
  - `round-robin`：默认轮询
  - `least_conn`：最少连接数
  - `ip_hash`：会话保持
  - `random`：随机
- 健康检查：
  - 被动检查：`max_fails` + `fail_timeout`
  - 主动检查：使用 `nginx_upstream_check_module` 或商业版
- 后端服务器权重配置根据服务器性能设置
- 灰度发布使用 `split_clients` 或 `map` 按比例分流

## SSL/TLS 规范

- 协议版本：仅启用 TLSv1.2 和 TLSv1.3
- 加密套件：
  ```nginx
  ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384;
  ssl_prefer_server_ciphers on;
  ```
- HSTS：`add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;`
- 证书自动续期使用 Certbot / acme.sh
- HTTP 自动跳转 HTTPS
- OCSP Stapling 启用

## 静态资源规范

- 静态文件缓存：
  ```nginx
  location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff2)$ {
      expires 30d;
      add_header Cache-Control "public, immutable";
  }
  ```
- Gzip 压缩：
  ```nginx
  gzip on;
  gzip_types text/plain text/css application/json application/javascript text/xml;
  gzip_min_length 1024;
  gzip_comp_level 6;
  ```
- 静态资源目录禁止执行脚本
- 禁止目录列表：`autoindex off`

## 安全规范

- 隐藏版本号：`server_tokens off;`
- 安全响应头：
  - `X-Frame-Options: SAMEORIGIN`
  - `X-Content-Type-Options: nosniff`
  - `X-XSS-Protection: 1; mode=block`
  - `Content-Security-Policy`
- 禁止访问隐藏文件：`location ~ /\. { deny all; }`
- 请求体大小限制：`client_max_body_size`
- 限流配置：
  - `limit_req_zone`：请求速率限制
  - `limit_conn_zone`：并发连接限制
- IP 黑白名单：`allow` / `deny`

## 注释规范

- 每个 Server 块必须有中文注释说明对应的服务
- 每个 Location 块必须有中文注释说明用途
- 关键配置项必须添加中文注释
- 复杂的 rewrite 规则必须注释说明意图
- 禁止无意义注释

## 代码质量强制要求

- 生产环境必须启用 HTTPS
- 必须配置安全响应头
- 必须隐藏版本号
- 代理必须传递真实客户端 IP
- 超时参数必须合理配置
- 禁止使用 `root` 指令在 `location` 中指向敏感目录
- 配置变更后必须 `nginx -t` 验证语法
- 必须配置访问日志和错误日志

## 性能优化

- Worker 进程数：`auto`（与 CPU 核心数一致）
- Worker 连接数：`worker_connections 10240`
- 开启 `sendfile`、`tcp_nopush`、`tcp_nodelay`
- 连接保持：`keepalive_timeout 65`
- Upstream 长连接：`keepalive` 指令
- 静态资源开启缓存和压缩
- 日志缓冲：`access_log ... buffer=32k flush=5m`

## 日志规范

- 自定义日志格式包含：时间、客户端 IP、请求方法、URI、状态码、响应时间、上游地址
  ```nginx
  log_format api_log '$remote_addr - $remote_user [$time_local] '
                     '"$request" $status $body_bytes_sent '
                     '"$http_referer" "$http_user_agent" '
                     '$request_time $upstream_response_time $upstream_addr';
  ```
- 不同服务使用独立日志文件
- 日志轮转使用 logrotate

## 最佳实践

- 使用 `include` 拆分配置，保持主配置简洁
- 使用 `map` 指令实现条件逻辑
- 使用 `try_files` 处理前端路由（SPA 应用）
- 配置热重载：`nginx -s reload`
- 监控使用 `stub_status` + Prometheus exporter
