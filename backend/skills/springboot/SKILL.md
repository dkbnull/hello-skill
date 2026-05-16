---
name: "springboot"
description: "Spring Boot开发专家助手。当用户需要进行Spring Boot项目搭建、REST API开发、MyBatis Plus、Security或微服务开发时调用。"
---

# Spring Boot 开发技能

你是一位资深 Spring Boot 开发工程师。在协助 Spring Boot 项目时，请遵循以下规范。

## 分层架构

- 固定分层结构：Controller → Service（接口 + Impl） → Mapper → Entity
- 禁止跨层调用，Controller 不写业务逻辑
- 包路径规范：
  - controller：请求入口，使用 `@RestController`，负责请求响应，保持精简
  - service：业务接口
  - service.impl：业务实现，使用 `@Service`，承载业务逻辑
  - mapper：数据访问，使用 `@Mapper` 或 `@MapperScan`，继承 `BaseMapper<T>`
  - entity：实体类
  - dto：请求/响应 DTO
  - common：通用类（异常、枚举、统一结果）
  - util：工具类
- Model 层：分为 entity（`@TableName` 实体类）、dto（请求/响应 DTO）、mapstruct（MapStruct 映射器）
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
- 复杂查询（多表关联）在 Mapper XML 中编写自定义 SQL
- 批量操作使用 `saveBatch`、`updateBatchById` 等批量方法
- 逻辑删除通过全局配置自动处理，查询时自动过滤已删除数据

## 命名规范

- 类名：PascalCase（`UserService`、`OrderController`、`UserCreateRequest`）
- 抽象类命名以 Base 或 Abstract 开头（`BaseEntity`、`AbstractProcessor`）
- 异常类命名以 Exception 结尾（`BusinessException`、`ValidationException`）
- 方法名：camelCase（`getUserById`、`calculateTotalAmount`）
- 变量名：camelCase（`userList`、`orderCount`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`、`DEFAULT_PAGE_SIZE`）
- 包名：小写点分隔，统一使用单数形式（`com.example.project.service`）
- 测试类：`{类名}Test`（`UserServiceTest`）
- DTO 类命名：`{实体}{操作}Request/Response`（`UserCreateRequest`、`UserUpdateResponse`）
- Mapper 接口命名：`{实体}Mapper`（`UserMapper`）
- Service 接口与实现：`{实体}Service` / `{实体}ServiceImpl`
- boolean 类型变量不加 is 前缀（POJO/数据库字段场景）
- Service/DAO 层方法命名规约：
  - 获取单个对象：`get` 前缀（`getUserById`）
  - 获取列表：`list` 前缀（`listUsersByRole`）
  - 获取统计：`count` 前缀（`countActiveUsers`）
  - 插入：`save` / `insert` 前缀（`saveUser`）
  - 删除：`remove` / `delete` 前缀（`removeUserById`）
  - 修改：`update` 前缀（`updateUserRole`）
- 领域模型命名规约：
  - 数据对象：`XxxDO`（`UserDO`）
  - 数据传输对象：`XxxDTO`（`UserDTO`）
  - 展示对象：`XxxVO`（`UserVO`）
  - 查询对象：`XxxQuery`（`UserQuery`）
- 枚举类名带 Enum 后缀（`UserStatusEnum`），枚举成员全大写下划线（`ACTIVE`、`DISABLED`）

## 注释规范

- 所有类、接口必须有中文 Javadoc 注释，说明用途和职责
- 所有 public 方法必须有中文 Javadoc 注释，包含功能说明、`@param`、`@return`、`@throws`
- 复杂业务逻辑、核心算法必须添加中文行内注释说明意图
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步
- Mapper XML 中的自定义 SQL 必须添加中文注释说明查询用途
- 注释掉的代码应直接删除，版本管理由 Git 负责，禁止保留注释代码

## 接口与返回值规范

- 统一使用 RESTful 风格接口，API 版本化管理：`/api/v1/...`
- 使用自定义 `Result<T>` 统一返回，包含 code、msg、data
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

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 方法体长度不超过 80 行，超过必须拆分
- 方法参数不超过 5 个，超过使用对象封装
- 大括号不换行（K&R 风格），一行一条语句
- if/else/for/while/do 语句必须使用大括号，即使只有一行代码
- 类成员排列顺序：静态变量 → 实例变量 → 构造方法 → 公有方法 → 私有方法
- 方法内部定义的局部变量应在首次使用时声明，不要集中在方法顶部

## OOP 规约

- 所有覆写方法必须加 `@Override` 注解
- 构造方法里禁止加入业务逻辑，如有初始化逻辑应放在 `@PostConstruct` 或 `init` 方法中
- POJO 类必须写 `toString` 方法，便于排查问题
- 可变参数必须放在参数列表最后
- 避免通过类的对象引用来访问静态变量或方法，应直接使用类名访问
- `equals` 方法必须满足对称性、传递性、一致性，重写 `equals` 必须同时重写 `hashCode`
- 优先使用基本数据类型而非包装类型，避免自动拆箱导致的 NPE

## 集合处理

- `ArrayList` 的 `subList` 结果不可强转成 `ArrayList`，否则抛出 `ClassCastException`
- 使用 `Map` 的 `entrySet` 遍历键值对，`keySet` 仅遍历键
- `Map` 的 key 为自定义对象时必须重写 `hashCode` 和 `equals`
- foreach 循环里禁止进行元素的 `remove`/`add` 操作，应使用 `Iterator`
- `Arrays.asList()` 返回的集合不能使用 `add`/`remove`/`clear` 方法
- 集合转数组必须使用 `toArray(T[] array)`，避免类型转换异常
- 泛型通配符：`<? extends T>` 上界通配符不能 `add`，`<? super T>` 下界通配符不能 `get`

## 代码质量强制要求

- 禁止空指针：所有可能为 null 的返回值必须判空或使用 `Optional`，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量或枚举
  - 禁止：`if (status == 1)`
  - 正确：`if (status == UserStatusEnum.ACTIVE.getCode())`
- 禁止 `==` 比较对象值，字符串使用 `equals()`，对象使用 `Objects.equals()`
- 集合操作前必须判空，使用 `CollectionUtils.isEmpty()` 或 `CollUtil.isEmpty()`
- 数值计算注意溢出和精度，金额必须使用 `BigDecimal`，禁止使用 `float`/`double`
- 日期时间使用 `LocalDateTime`，禁止使用 `java.util.Date`
- 禁止在循环中拼接字符串，使用 `StringBuilder`
- 禁止在循环中执行数据库操作，使用批量方法
- switch 语句必须包含 default 分支
- 所有资源（流、连接）必须使用 try-with-resources 关闭
- 并发场景必须使用线程安全集合或加锁，禁止在多线程中使用非安全集合
- 优先使用 JDK8+ 特性：Stream、Lambda、Optional
- 方法入参必须校验：外部传入参数、跨层调用参数、public 方法入参
- 优先使用卫语句减少 if-else 嵌套，尽早 return

## 并发处理

- 线程资源必须通过线程池提供，禁止显式 `new Thread()`
- 线程池禁止使用 `Executors` 创建，必须通过 `ThreadPoolExecutor` 方式，明确线程池参数
- 必须回收自定义 `ThreadLocal` 变量，使用 `try-finally` 调用 `remove()`，避免内存泄漏
- 多线程加锁必须保持一致的加锁顺序，避免死锁
- `SimpleDateFormat` 线程不安全，使用 `DateTimeFormatter` 替代
- 并发修改同一记录时必须加锁，避免数据不一致
- `volatile` 解决多线程内存可见性问题，但不能保证原子性，原子操作使用 `Atomic` 类

## 最佳实践

- 始终使用构造器注入（`@RequiredArgsConstructor`）
- 使用 DTO 进行请求/响应，绝不直接暴露实体类
- 读操作使用 `@Transactional(readOnly = true)`
- 使用 MapStruct 进行实体与 DTO 的映射
- 使用 Spring Security + JWT 实现认证授权
- 非阻塞操作使用 `@Async`
- 启用 Actuator 端点进行应用监控
