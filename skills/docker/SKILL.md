---
name: "docker"
description: "Docker开发专家助手。当用户需要进行Docker镜像构建、容器编排、Dockerfile编写、Docker Compose或容器化部署时调用。"
---

# Docker 开发技能

你是一位资深 Docker 开发工程师。在协助 Docker 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 Docker 24.0+ 版本
- 使用 BuildKit 构建镜像（`DOCKER_BUILDKIT=1`）
- 生产环境禁止使用 `latest` 标签，必须指定明确版本号
- 禁止在容器中运行 SSH 服务
- 禁止在容器中存储持久化数据，必须使用 Volume

## 命名规范

- 镜像名：小写 + 短横线分隔（`user-service`、`order-api`）
- 镜像标签格式：`{版本号}-{环境}`（`1.0.0-prod`、`1.0.0-dev`）
- 容器名：小写 + 短横线分隔（`user-service-prod`、`redis-master`）
- Volume 名：小写 + 短横线分隔（`user-service-data`、`mysql-data`）
- 网络名：小写 + 短横线分隔（`backend-network`、`frontend-network`）
- 命名语义化，禁止拼音、无意义缩写

## Dockerfile 规范

- 基础镜像选择：
  - 优先使用 Alpine 版本（`node:18-alpine`、`openjdk:17-jdk-alpine`）
  - 禁止使用 `latest` 标签，必须指定明确版本
  - 优先使用官方镜像，禁止使用来源不明的第三方镜像
- 多阶段构建：
  - 第一阶段：编译构建（使用完整镜像）
  - 第二阶段：运行时（使用精简镜像），只复制构建产物
- 指令顺序优化：
  - 变化频率低的指令放前面（FROM、WORKDIR、RUN apt-get）
  - 变化频率高的指令放后面（COPY 源码、CMD）
  - 充分利用 Docker 缓存层，减少构建时间
- COPY 规范：
  - 先 COPY 依赖文件（package.json、pom.xml），再 COPY 源码
  - 使用 `.dockerignore` 排除不需要的文件（node_modules、.git、dist）
- RUN 规范：
  - 多条命令合并为一个 RUN，减少镜像层数
  - 安装后清理缓存：`&& rm -rf /var/lib/apt/lists/*`
- 安全规范：
  - 禁止以 root 用户运行应用，创建专用用户
  - 使用 `USER appuser` 切换用户
  - 禁止在镜像中硬编码敏感信息（密码、密钥）

## Dockerfile 模板

### Java/SpringBoot
```dockerfile
FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml ./
COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Node.js
```dockerfile
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .

FROM node:18-alpine
WORKDIR /app
COPY --from=build /app ./
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
EXPOSE 3000
CMD ["node", "src/index.js"]
```

## Docker Compose 规范

- 使用 `docker-compose.yml`（v3.8+ 语法）
- 服务命名语义化，与环境对应
- 环境变量使用 `.env` 文件管理，禁止硬编码
- Volume 挂载必须指定读写权限（`:ro` 只读、`:rw` 读写）
- 网络隔离：前后端分属不同网络
- 健康检查必须配置：`healthcheck`
- 资源限制必须配置：`deploy.resources.limits`
- 依赖关系使用 `depends_on` + `condition: service_healthy`

## 镜像管理规范

- 镜像大小优化：
  - 使用多阶段构建
  - 使用 Alpine 基础镜像
  - 合并 RUN 指令减少层数
  - 清理包管理器缓存
- 镜像安全：
  - 定期扫描镜像漏洞（Trivy、Snyk）
  - 及时更新基础镜像版本
  - 禁止在镜像中包含调试工具（curl、vim）
- 镜像仓库：
  - 私有仓库使用 Harbor 或云厂商 ACR/ECR
  - 推送标签必须包含版本号和 Git commit hash

## 代码质量强制要求

- 禁止在 Dockerfile 中硬编码敏感信息
- 禁止以 root 用户运行应用容器
- 禁止使用 `latest` 标签部署生产环境
- 容器必须设置健康检查
- 容器必须设置资源限制（CPU、内存）
- 日志必须输出到 stdout/stderr，禁止写文件
- 容器必须能无状态水平扩展，状态外置到 Redis/数据库

## 最佳实践

- 使用 `.dockerignore` 排除无关文件
- 使用 `docker system prune` 定期清理无用资源
- 开发环境使用 Volume 挂载实现热重载
- 生产环境使用非绑定挂载（named volume）
- 使用 `docker stats` 监控容器资源使用
- 日志收集使用 ELK 或 Loki，禁止依赖容器内文件
