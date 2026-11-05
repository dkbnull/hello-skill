---
name: "springboot"
description: "Spring Boot开发专家助手。当用户需要进行Spring Boot项目搭建、REST API开发、MyBatis Plus或Security开发时调用。"
---

# Spring Boot 开发技能

你是一位资深 Spring Boot 开发工程师。在协助 Spring Boot 项目时，请遵循以下规范。

> Java 语言规范遵循 languages/skills/java，本技能仅补充 Spring Boot 特有内容。

## 分层架构

- 固定分层结构：Controller → Service（接口 + Impl） → Repository → Mapper → Entity
- 禁止跨层调用，Controller 不写业务逻辑，Service 禁止直接注入或调用 Mapper
- 所有数据库读写封装在 Repository 层，Service 只依赖 Repository，Repository 内部调用 Mapper
- 包路径规范：
  - controller：请求入口，使用 `@RestController`，负责请求响应，保持精简
  - service：业务接口
  - service.impl：业务实现，使用 `@Service`，承载业务逻辑，只依赖 Repository
  - repository：数据访问封装，使用 `@Repository`，内部注入 Mapper，对外提供数据读写方法
  - mapper：MyBatis Plus Mapper 接口，使用 `@Mapper` 或 `@MapperScan`，继承 `BaseMapper<T>`，仅在 Repository 内部使用
  - entity：实体类
  - dto：请求/响应 DTO
  - common：通用类（异常、枚举、统一结果）
  - mapstruct：MapStruct 映射器
  - util：工具类
- Exception 层：使用 `@RestControllerAdvice` 全局异常处理
- Security 层：安全配置和认证授权

## 核心依赖

- spring-boot-starter-web
- mybatis-plus-spring-boot3-starter
- mybatis-plus-boot-starter（SpringBoot 2.x 使用）
- spring-boot-starter-validation
- spring-boot-starter-security
- spring-boot-starter-actuator
- lombok
- mapstruct
- mybatis-plus 的 SQL 自动填充

## MyBatis Plus 配置要点

- 数据源配置使用环境变量注入敏感信息
- 配置 `mybatis-plus.mapper-locations` 指定 XML 映射文件路径（如需自定义 SQL）
- 配置 `mybatis-plus.type-aliases-package` 指定实体类包路径
- 配置 `mybatis-plus.configuration.map-underscore-to-camel-case: true` 开启驼峰命名映射
- 配置 `mybatis-plus.global-config.db-config.logic-delete-field` 逻辑删除字段
- 禁止使用 `ddl-auto`，数据库迁移使用 Flyway/Liquibase
- 合理配置日志级别

## MyBatis Plus 实体规范

- 实体类使用 `@TableName` 注解映射表名
- 主键字段使用 `@TableId` 注解，指定主键策略
- 非数据库字段使用 `@TableField(exist = false)` 标注
- 逻辑删除字段使用 `@TableLogic` 注解
- 自动填充字段（创建时间、更新时间）使用 `@TableField(fill = FieldFill.INSERT)` 或 `FieldFill.INSERT_UPDATE`
- 实现 `MetaObjectHandler` 接口处理自动填充
- 实体类使用 Lombok：`@Data`、`@NoArgsConstructor`、`@AllArgsConstructor`

## MyBatis Plus 查询规范

- 简单查询使用 `QueryWrapper` 或 `LambdaQueryWrapper`，优先使用 Lambda 避免硬编码字段名
- 分页查询使用 `MybatisPlusInterceptor` 配置 `PaginationInnerInterceptor` 分页插件
- 使用 `Page<T>` 对象进行分页，通过 `IPage<T>` 返回分页结果
- 复杂查询（多表关联）在 Mapper XML 中编写自定义 SQL，通过 Repository 封装后供 Service 调用
- 批量操作使用 `saveBatch`、`updateBatchById` 等批量方法，在 Repository 层封装
- 逻辑删除通过全局配置自动处理，查询时自动过滤已删除数据

## 命名规范

- 基础命名（类名 PascalCase、方法/变量 camelCase、常量 UPPER_SNAKE_CASE、包名单数形式、测试类 `{类名}Test`）遵循 java 技能
- 抽象类命名以 Base 或 Abstract 开头（`BaseEntity`）
- 异常类命名以 Exception 结尾（`BusinessException`）
- DTO 类命名（按场景区分，禁止混用）：
  - 请求入参：`{实体}{操作}Request`（`UserCreateRequest`、`UserUpdateRequest`、`UserLoginRequest`）
  - 接口响应：`{实体}{操作}Response`（`UserCreateResponse`、`UserUpdateResponse`、`UserLoginResponse`）
  - 通用展示对象（不区分操作类型时使用）：`XxxVO`（`UserVO`），仅用于列表/详情等无操作语义的查询返回
  - 跨服务传输对象：`XxxDTO`（`UserDTO`），仅用于服务间调用，Controller 层禁止使用
  - 查询条件封装：`XxxQuery`（`UserQuery`），仅用于分页/条件查询的请求参数
  - 数据对象：`XxxDO`（`UserDO`），仅用于 Repository/Mapper 层，与数据库表映射
  - **关键约束**：Controller 层接口返回值统一使用 `{实体}{操作}Response` 或 `XxxVO`
- Mapper 接口命名：`{实体}Mapper`（`UserMapper`）
- Repository 类命名：`{实体}Repository`
- Service 接口与实现：`{实体}Service` / `{实体}ServiceImpl`
- boolean 类型变量不加 is 前缀（POJO/数据库字段场景）
- Service/Repository 层方法命名规约：
  - 获取单个对象：`get` 前缀（`getUserById`）
  - 获取列表：`list` 前缀（`listUsersByRole`）
  - 获取统计：`count` 前缀（`countActiveUsers`）
  - 插入：`save` / `insert` 前缀（`saveUser`）
  - 删除：`remove` / `delete` 前缀（`removeUserById`）
  - 修改：`update` 前缀（`updateUserRole`）

- 枚举类名带 Enum 后缀（`UserStatusEnum`），枚举成员全大写下划线（`ACTIVE`、`DISABLED`）

## 注释规范

- Javadoc、TODO 格式、注释同步等通用注释规范遵循 java 技能
- Mapper XML 中的自定义 SQL 必须添加中文注释说明查询用途

## 接口与返回值规范

- 统一使用 RESTful 风格接口，API 版本化管理：`/api/v1/...`
- 使用自定义 `Result<T>` 统一返回，包含 code、message、data，成功码固定为 0，失败使用5位分段编码（如 10001、20001），绝大部分接口返回 HTTP 200；错误码分段规则与响应结构详见 api-design 技能。
- Controller 接口添加 Swagger/OpenAPI 注解：`@Tag`、`@Operation`
- 请求参数接收：
  - JSON 请求体使用 `@RequestBody`
  - 查询参数使用 `@RequestParam`
  - 路径参数使用 `@PathVariable`
- 参数校验必须完善：`@NotBlank`、`@NotNull`、`@Size`、`@Valid` 等

## 日志规范

- 使用 `@Slf4j`（Lombok）+ Logback，禁止使用 `System.out.println()`
- 禁止使用 `e.printStackTrace()`，必须通过日志框架记录异常
- 日志信息必须使用中文，参数化格式：`log.info("用户 {} 登录成功", userId)`
- 日志级别使用规范：
  - ERROR：系统异常、不可恢复错误，必须附带完整异常栈
  - WARN：潜在问题、业务异常、降级处理
  - INFO：关键业务节点（用户登录、订单创建、支付完成等）
  - DEBUG：调试信息，生产环境关闭
- 绝不记录敏感数据（密码、令牌、身份证号、银行卡号）
- 异常日志必须包含上下文信息：`log.error("创建订单失败，用户ID：{}", userId, e)`
- 生产环境禁止使用 DEBUG 级别日志输出

## 异常规范

- 自定义异常必须使用中文错误信息
- 建立异常层次结构：`BaseException -> NotFoundException, ValidationException, BusinessException`
- 异常类必须包含错误码（code）和中文错误信息（message）
- 业务校验失败抛出 `BusinessException`，资源不存在抛出 `NotFoundException`
- 使用 `@RestControllerAdvice` 做全局异常捕获和处理
- 禁止在 Controller 层捕获异常后返回错误响应，统一由全局异常处理器处理
- `@Transactional` 必须指定 `rollbackFor = Exception.class`
- 捕获异常时禁止空 catch 块，至少记录日志
- 返回类型为基本数据类型时，return 包装数据类型对象可能自动拆箱产生 NPE，必须判空
- 集合元素即使 `isNotEmpty`，取出的数据元素也可能为 null，使用前必须判空

## 格式规范

- 格式规范（缩进、行宽、方法长度、参数个数、大括号风格等）遵循 java 技能

## 代码质量强制要求

- 通用代码质量要求（空指针、魔法值、equals、BigDecimal、日期时间、循环拼接、资源关闭、并发安全等）遵循 java 技能
- 集合操作前必须判空，使用 `CollectionUtils.isEmpty()` 或 `CollUtil.isEmpty()`
- 禁止在循环中执行数据库操作，使用批量方法

## 最佳实践

- 始终使用构造器注入（`@RequiredArgsConstructor`）
- 使用 DTO 进行请求/响应，绝不直接暴露实体类
- 读操作使用 `@Transactional(readOnly = true)`
- 使用 Spring Security + JWT 实现认证授权
- 非阻塞操作使用 `@Async`
- 启用 Actuator 端点进行应用监控
