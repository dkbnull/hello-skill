<h1 align="center">
  Hello Skill
</h1>

<p align="center">
  <a href="README_EN.md">English</a> | <a href="README.md">简体中文</a>
</p>

<p align="center">
  <a href="https://github.com/dkbnull/hello-skill" target="_blank">
     <img src="https://img.shields.io/badge/GitHub-Repo-blue?logo=github">
  </a>
  <a href="https://gitee.com/dkbnull/hello-skill" target="_blank">
     <img src="https://img.shields.io/badge/Gitee-Repo-red?logo=gitee">
  </a>
  <img src="https://img.shields.io/badge/License-MIT-yellow">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Common_Rules-2-ff69b4">
  <img src="https://img.shields.io/badge/Common_Skills-20-9cf">
  <img src="https://img.shields.io/badge/Programming_Languages-33-blueviolet">
  <img src="https://img.shields.io/badge/Backend_Frameworks-13-blue">
  <img src="https://img.shields.io/badge/Frontend_Frameworks-17-green">
  <img src="https://img.shields.io/badge/Mobile_&_Desktop-15-orange">
  <img src="https://img.shields.io/badge/Databases-17-red">
  <img src="https://img.shields.io/badge/Middleware-13-yellow">
  <img src="https://img.shields.io/badge/DevOps_Tools-12-inactive">
  <img src="https://img.shields.io/badge/AI_&_LLM-9-ff69b4">
  <img src="https://img.shields.io/badge/Data_Engineering-4-9cf">
  <img src="https://img.shields.io/badge/Total-153-brightgreen">
</p>

---

## Introduction

This project is used to store and manage SKILL.md and RULE files, providing Skill configurations and common Rule definitions for various tech stacks.

> **Usage**:
> - **Skill**: Go to the desired category directory and copy the contents of the `skills/` folder into `.trae/skills/`.
> - **Rule**: Copy the rule files from `common/rules/` into `.trae/rules/`.

## Rules

### Common Rules

| Rule | Description |
|------|-------------|
| [Chinese Response & Auto Skill Loading](common/rules/chinese-with-auto-skill.md) | Enforce Chinese responses and automatically load the corresponding SKILL based on task context |
| [Coding Discipline](common/rules/coding-discipline.md) | Four behavioral principles — think before coding, conciseness first, surgical changes, goal-driven validation — to reduce common LLM coding mistakes. Adapted from [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) |

## Common Skills

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Code Generation](common/skills/code-generation/SKILL.md) | AI-assisted coding standards to reduce hallucinations and logic omissions | — |
| [Requirement Analysis](common/skills/requirement-analysis/SKILL.md) | Pre-coding requirement analysis to eliminate ambiguity and omissions | — |
| [Debugging](common/skills/debug/SKILL.md) | Systematic debugging methodology for quickly locating root causes | — |
| [Code Review](common/skills/code-review/SKILL.md) | Multi-dimensional code review to reduce AI-generated code errors | — |
| [Testing](common/skills/testing/SKILL.md) | Systematic testing methodology covering unit tests, integration tests, E2E tests, and TDD practices | — |
| [Performance Optimization](common/skills/performance/SKILL.md) | Systematic performance analysis with full-stack optimization strategies | — |
| [Refactoring](common/skills/refactoring/SKILL.md) | Systematic refactoring methodology to improve code quality | — |
| [Error Handling](common/skills/error-handling/SKILL.md) | Systematic error handling ensuring exceptions are detectable, traceable, and recoverable | — |
| [Defensive Programming](common/skills/defensive-programming/SKILL.md) | Proactively anticipate errors and write robust defensive code | — |
| [Concise Code](common/skills/concise-code/SKILL.md) | Eliminate duplication, redundancy, and wheel reinvention in AI-generated code, control code volume | — |
| [Security Review](common/skills/security-review/SKILL.md) | Security scanning covering injection attacks, authentication & authorization, data security, configuration security, and dependency security | — |
| [API Design](common/skills/api-design/SKILL.md) | Systematic API design standards to reduce design flaws | — |
| [System Architecture](common/skills/architecture/SKILL.md) | Architecture methodology covering layered architecture, microservices, event-driven architecture, and ADRs | — |
| [Design Patterns](common/skills/design-patterns/SKILL.md) | Problem-driven pattern selection for reusability and extensibility | — |
| [UI/UX Design](common/skills/ui-ux-design/SKILL.md) | UI design, interaction design, and user experience optimization standards | — |
| [Technical Documentation](common/skills/tech-doc/SKILL.md) | Standardized documentation writing guidelines ensuring clarity, completeness, and maintainability | — |
| [Observability](common/skills/observability/SKILL.md) | Observability methodology: metrics, logging, distributed tracing, and alerting | — |
| [Concurrency Design](common/skills/concurrency/SKILL.md) | Concurrency and async design: thread pools, locks, backpressure, and idempotency methodology | — |
| [Structured Thinking](common/skills/structured-thinking/SKILL.md) | Systematic thinking framework to reduce AI hallucinations and logic errors | — |
| [Role Switching](common/skills/role-switch/SKILL.md) | Automatically switch professional roles based on context during coding | — |

## Tech Stack Skills

### Programming Languages

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Java](languages/skills/java/SKILL.md) | Java language development | — |
| [Python](languages/skills/python/SKILL.md) | Python language development | — |
| [JavaScript](languages/skills/javascript/SKILL.md) | JavaScript development (including browser-side/DOM standards) | — |
| [TypeScript](languages/skills/typescript/SKILL.md) | TypeScript type-safe development | — |
| [Go](languages/skills/go/SKILL.md) | Go language development | — |
| [C++](languages/skills/cpp/SKILL.md) | C++ language development | — |
| [C# / .NET](languages/skills/csharp/SKILL.md) | C# / .NET development | — |
| [C](languages/skills/c/SKILL.md) | C language development | — |
| [Rust](languages/skills/rust/SKILL.md) | Rust language development | — |
| [PHP](languages/skills/php/SKILL.md) | PHP backend development | — |
| [Kotlin](languages/skills/kotlin/SKILL.md) | Kotlin language development | — |
| [Swift](languages/skills/swift/SKILL.md) | Swift / Apple ecosystem development | ios (SwiftUI/testing standards) |
| [Ruby](languages/skills/ruby/SKILL.md) | Ruby / Rails development | — |
| [Scala](languages/skills/scala/SKILL.md) | Scala functional + big data development | — |
| [Dart](languages/skills/dart/SKILL.md) | Dart language development | — |
| [Shell](languages/skills/shell/SKILL.md) | Shell / Bash script development | — |
| [Lua](languages/skills/lua/SKILL.md) | Lua scripting / game scripting | — |
| [Haskell](languages/skills/haskell/SKILL.md) | Haskell pure functional development | — |
| [Elixir](languages/skills/elixir/SKILL.md) | Elixir high-concurrency development | — |
| [Erlang](languages/skills/erlang/SKILL.md) | Erlang / OTP high-concurrency fault-tolerant development | — |
| [R](languages/skills/r/SKILL.md) | R statistical analysis development | — |
| [MATLAB](languages/skills/matlab/SKILL.md) | MATLAB numerical computing development | — |
| [Julia](languages/skills/julia/SKILL.md) | Julia scientific computing development | — |
| [Perl](languages/skills/perl/SKILL.md) | Perl scripting development | — |
| [Visual Basic](languages/skills/vb/SKILL.md) | Visual Basic / VBA development | — |
| [Delphi](languages/skills/delphi/SKILL.md) | Delphi desktop application development | — |
| [Fortran](languages/skills/fortran/SKILL.md) | Fortran scientific computing development | — |
| [Assembly](languages/skills/assembly/SKILL.md) | Assembly language development | — |
| [COBOL](languages/skills/cobol/SKILL.md) | COBOL enterprise development | — |
| [Objective-C](languages/skills/objective-c/SKILL.md) | Objective-C development and mixed compilation with Swift | — |
| [Groovy](languages/skills/groovy/SKILL.md) | Groovy / Gradle scripting development | — |
| [PowerShell](languages/skills/powershell/SKILL.md) | PowerShell script development | — |
| [Scratch](languages/skills/scratch/SKILL.md) | Scratch visual programming | — |

### Backend Frameworks

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Spring Boot](backend/skills/springboot/SKILL.md) | Spring Boot backend development | java |
| [Spring Cloud](backend/skills/springcloud/SKILL.md) | Spring Cloud microservices development | java, springboot |
| [Django](backend/skills/django/SKILL.md) | Django web framework development | python |
| [FastAPI](backend/skills/fastapi/SKILL.md) | FastAPI framework development | python |
| [Flask](backend/skills/flask/SKILL.md) | Flask lightweight web development | python |
| [Gin](backend/skills/gin/SKILL.md) | Gin high-performance HTTP framework development | go |
| [Node.js](backend/skills/nodejs/SKILL.md) | Node.js backend development | express |
| [Express](backend/skills/express/SKILL.md) | Express web development | nodejs, javascript |
| [NestJS](backend/skills/nestjs/SKILL.md) | NestJS enterprise-grade backend development | — |
| [Laravel](backend/skills/laravel/SKILL.md) | Laravel PHP backend development | — |
| [gRPC](backend/skills/grpc/SKILL.md) | gRPC microservices communication | corresponding language skill |
| [Quarkus](backend/skills/quarkus/SKILL.md) | Quarkus cloud-native Java development and GraalVM native compilation | java |
| [Ktor](backend/skills/ktor/SKILL.md) | Ktor Kotlin lightweight server-side development | kotlin |

### Frontend Frameworks

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [HTML](frontend/skills/html/SKILL.md) | HTML page structure and semantic development | css, javascript (combine as needed) |
| [CSS](frontend/skills/css/SKILL.md) | CSS styling, responsive layout, and animation development | — |
| [Vue](frontend/skills/vue/SKILL.md) | Vue frontend development (including TS/Vite integration conventions) | typescript, vite (combine as needed) |
| [React](frontend/skills/react/SKILL.md) | React frontend development | — |
| [Angular](frontend/skills/angular/SKILL.md) | Angular enterprise-grade frontend development | — |
| [Next.js](frontend/skills/nextjs/SKILL.md) | Next.js full-stack framework development | — |
| [Nuxt](frontend/skills/nuxt/SKILL.md) | Nuxt full-stack SSR / SSG development | — |
| [Svelte](frontend/skills/svelte/SKILL.md) | Svelte compiled framework development | — |
| [Svelte Kit](frontend/skills/svelte-kit/SKILL.md) | SvelteKit full-stack framework development | — |
| [Astro](frontend/skills/astro/SKILL.md) | Astro content-driven website development | — |
| [Tailwind CSS](frontend/skills/tailwindcss/SKILL.md) | Tailwind CSS utility-first styling development | — |
| [Vite](frontend/skills/vite/SKILL.md) | Vite build tool development | — |
| [Webpack](frontend/skills/webpack/SKILL.md) | Webpack build configuration and optimization | corresponding framework skill |
| [Layui](frontend/skills/layui/SKILL.md) | Layui frontend framework development | — |
| [Ant Design](frontend/skills/antd/SKILL.md) | Ant Design React component library | react |
| [Element Plus](frontend/skills/element-plus/SKILL.md) | Element Plus Vue 3 component library | vue |
| [Jest / Vitest](frontend/skills/jest-vitest/SKILL.md) | Frontend unit testing frameworks | testing |

### Mobile & Desktop

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Android (Kotlin)](mobile-desktop/skills/android-kotlin/SKILL.md) | Android Kotlin application development | kotlin |
| [iOS](mobile-desktop/skills/ios/SKILL.md) | iOS application development | swift |
| [Flutter](mobile-desktop/skills/flutter/SKILL.md) | Flutter application development | dart |
| [React Native](mobile-desktop/skills/react-native/SKILL.md) | React Native cross-platform mobile development | react |
| [WeChat Mini Program](mobile-desktop/skills/miniprogram/SKILL.md) | Mini program development: WeChat-focused, covering Alipay and Douyin platform differences | — |
| [HarmonyOS](mobile-desktop/skills/harmonyos/SKILL.md) | HarmonyOS native application development (ArkTS + ArkUI) | — |
| [Electron](mobile-desktop/skills/electron/SKILL.md) | Electron desktop application development | — |
| [Compose Multiplatform](mobile-desktop/skills/compose-multiplatform/SKILL.md) | Kotlin multiplatform UI development | kotlin, android-kotlin |
| [Qt](mobile-desktop/skills/qt/SKILL.md) | Qt cross-platform desktop application development | — |
| [JavaFX](mobile-desktop/skills/javafx/SKILL.md) | JavaFX desktop application development | java |
| [WPF](mobile-desktop/skills/wpf/SKILL.md) | WPF Windows desktop application development | csharp |
| [.NET MAUI](mobile-desktop/skills/maui/SKILL.md) | .NET MAUI cross-platform application development | csharp |
| [Android (Java)](mobile-desktop/skills/android-java/SKILL.md) | Android Java application development (traditional View system) | android-kotlin |
| [UniApp](mobile-desktop/skills/uniapp/SKILL.md) | UniApp cross-platform development | — |
| [Tauri](mobile-desktop/skills/tauri/SKILL.md) | Tauri desktop application development | — |

### Databases

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [MySQL](database/skills/mysql/SKILL.md) | MySQL database development | sql |
| [Redis](database/skills/redis/SKILL.md) | Redis caching and data structures | — |
| [PostgreSQL](database/skills/postgresql/SKILL.md) | PostgreSQL database development | sql |
| [SQL Server](database/skills/sqlserver/SKILL.md) | SQL Server database development | sql |
| [Oracle](database/skills/oracle/SKILL.md) | Oracle database development | sql |
| [MongoDB](database/skills/mongodb/SKILL.md) | MongoDB document database development | — |
| [Elasticsearch](database/skills/elasticsearch/SKILL.md) | Elasticsearch search engine development | — |
| [SQLite](database/skills/sqlite/SKILL.md) | SQLite embedded database development | — |
| [SQL](database/skills/sql/SKILL.md) | SQL standard query language (authoritative common standard) | — |
| [ClickHouse](database/skills/clickhouse/SKILL.md) | ClickHouse OLAP database development | — |
| [TiDB](database/skills/tidb/SKILL.md) | TiDB distributed NewSQL database development | mysql |
| [TDengine](database/skills/tdengine/SKILL.md) | TDengine time-series database development | — |
| [Neo4j](database/skills/neo4j/SKILL.md) | Neo4j graph database development | — |
| [Doris](database/skills/doris/SKILL.md) | Apache Doris OLAP analytical database development | mysql (partial syntax compatibility) |
| [InfluxDB](database/skills/influxdb/SKILL.md) | InfluxDB time-series database development | — |
| [Database Functions](database/skills/db-function/SKILL.md) | Database custom function development | — |
| [Stored Procedures](database/skills/db-procedure/SKILL.md) | Database stored procedure development | — |

### Middleware

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Service Discovery Common](middleware/skills/service-discovery/SKILL.md) | Common standards for service registration/discovery, health checks, and configuration centers (authoritative) | — |
| [Nginx](middleware/skills/nginx/SKILL.md) | Nginx reverse proxy and load balancing | — |
| [Kafka](middleware/skills/kafka/SKILL.md) | Kafka message queue development (authoritative common standard for message queues) | — |
| [RabbitMQ](middleware/skills/rabbitmq/SKILL.md) | RabbitMQ message middleware development | kafka |
| [RocketMQ](middleware/skills/rocketmq/SKILL.md) | RocketMQ message queue development | kafka |
| [Dubbo](middleware/skills/dubbo/SKILL.md) | Dubbo microservices RPC development | — |
| [Nacos](middleware/skills/nacos/SKILL.md) | Nacos service registration/discovery and configuration center | service-discovery |
| [ZooKeeper](middleware/skills/zookeeper/SKILL.md) | ZooKeeper distributed coordination | service-discovery |
| [Sentinel](middleware/skills/sentinel/SKILL.md) | Sentinel traffic governance and circuit breaking/degradation | — |
| [Consul](middleware/skills/consul/SKILL.md) | Consul service discovery and configuration management | service-discovery |
| [etcd](middleware/skills/etcd/SKILL.md) | etcd distributed key-value storage | service-discovery |
| [Pulsar](middleware/skills/pulsar/SKILL.md) | Pulsar cloud-native message queue | kafka |
| [Vault](middleware/skills/vault/SKILL.md) | Vault secrets and sensitive data management | — |

### DevOps & Tools

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Git](devops/skills/git/SKILL.md) | Git version control: branching models, commit standards, and team collaboration workflows | — |
| [Docker](devops/skills/docker/SKILL.md) | Docker containerized deployment | — |
| [Jenkins](devops/skills/jenkins/SKILL.md) | Jenkins CI/CD pipelines | ci-cd, groovy |
| [Kubernetes](devops/skills/kubernetes/SKILL.md) | Kubernetes container orchestration | docker, helm |
| [GitHub Actions](devops/skills/github-actions/SKILL.md) | GitHub Actions CI/CD pipelines | ci-cd |
| [GitLab CI](devops/skills/gitlab-ci/SKILL.md) | GitLab CI/CD pipelines | ci-cd |
| [Helm](devops/skills/helm/SKILL.md) | Helm Chart application packaging and management | docker |
| [Terraform](devops/skills/terraform/SKILL.md) | Terraform infrastructure as code | — |
| [Ansible](devops/skills/ansible/SKILL.md) | Ansible automated configuration management | — |
| [Prometheus](devops/skills/prometheus/SKILL.md) | Prometheus monitoring and alerting | — |
| [Grafana](devops/skills/grafana/SKILL.md) | Grafana visualization and alerting platform | — |
| [CI/CD Common](devops/skills/ci-cd/SKILL.md) | Common standards for CI/CD pipelines (authoritative) | — |

### AI / LLM

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Prompt Engineering](ai/skills/prompt-engineering/SKILL.md) | Prompt design optimization and LLM prompt development | — |
| [RAG](ai/skills/rag/SKILL.md) | RAG retrieval-augmented generation system design | — |
| [LangChain](ai/skills/langchain/SKILL.md) | LangChain application development and RAG retrieval-augmented generation | — |
| [MCP](ai/skills/mcp/SKILL.md) | Model Context Protocol development | — |
| [Training Common](ai/skills/training/SKILL.md) | Common standards for deep learning training engineering (authoritative) | — |
| [PyTorch](ai/skills/pytorch/SKILL.md) | PyTorch deep learning development and model training | training |
| [Hugging Face](ai/skills/huggingface/SKILL.md) | Hugging Face model hub and Transformers development | training |
| [TensorFlow](ai/skills/tensorflow/SKILL.md) | TensorFlow deep learning development and model deployment | training |
| [Fine-tuning](ai/skills/finetune/SKILL.md) | LLM fine-tuning: LoRA/QLoRA and data engineering | training |

### Data Engineering

| Skill | Description | Dependencies |
|-------|-------------|--------------|
| [Spark](data-engineering/skills/spark/SKILL.md) | Spark big data computing and batch processing | scala |
| [Flink](data-engineering/skills/flink/SKILL.md) | Flink real-time stream computing and pipeline development | — |
| [Hadoop](data-engineering/skills/hadoop/SKILL.md) | Hadoop big data storage and computing | — |
| [Airflow](data-engineering/skills/airflow/SKILL.md) | Airflow workflow scheduling and ETL automation | — |
