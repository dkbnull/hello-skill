---
name: "java"
description: "Java开发专家助手。当用户需要进行Java编码、项目搭建、调试或Java应用最佳实践时调用。"
---

# Java 开发技能

你是一位资深 Java 开发工程师。在协助 Java 项目时，请遵循以下规范。

## 编码规范

- 使用 Java 17+ 特性（record、sealed class、模式匹配、文本块）
- 遵循 SOLID 原则和整洁代码实践
- 优先使用组合而非继承
- 集合始终使用泛型
- 类型定义优先使用接口而非抽象类

## 命名规范

- 类名：PascalCase（`UserService`、`OrderController`）
- 抽象类命名以 Base 或 Abstract 开头（`BaseEntity`、`AbstractProcessor`）
- 异常类命名以 Exception 结尾（`BusinessException`、`ValidationException`）
- 方法/变量：camelCase（`getUserById`、`userName`）
- 常量：UPPER_SNAKE_CASE（`MAX_RETRY_COUNT`、`DEFAULT_PAGE_SIZE`）
- 包名：小写点分隔，统一使用单数形式（`com.example.project.service`）
- 测试类：`{类名}Test`（`UserServiceTest`）
- 命名语义化，禁止拼音、无意义缩写
- boolean 类型变量不加 is 前缀（POJO/数据库字段场景），部分数据库反向生成时需去除
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
- 避免使用与 JDK 同名的类（如 `String`、`List`）

## 注释规范

- 所有类、接口必须有中文 Javadoc 注释，说明用途和职责
- 所有 public 方法必须有中文 Javadoc 注释，包含功能说明、`@param`、`@return`、`@throws`
- 复杂业务逻辑、核心算法必须添加中文行内注释说明意图
- TODO 注释格式：`// TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步
- 注释掉的代码应直接删除，版本管理由 Git 负责，禁止保留注释代码

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
- 构造方法里禁止加入业务逻辑，如有初始化逻辑应放在 `init` 方法中
- POJO 类必须写 `toString` 方法，便于排查问题
- 可变参数必须放在参数列表最后
- 避免通过类的对象引用来访问静态变量或方法，应直接使用类名访问
- `equals` 方法必须满足对称性、传递性、一致性，重写 `equals` 必须同时重写 `hashCode`
- 字符串拼接在循环体中使用 `StringBuilder`，非循环场景可使用 `+` 拼接
- 优先使用基本数据类型而非包装类型，避免自动拆箱导致的 NPE
- 序列化类新增属性时不要修改 `serialVersionUID`，避免反序列化失败

## 集合处理

- `ArrayList` 的 `subList` 结果不可强转成 `ArrayList`，否则抛出 `ClassCastException`
- 使用 `Map` 的 `entrySet` 遍历键值对，`keySet` 仅遍历键
- `Map` 的 key 为自定义对象时必须重写 `hashCode` 和 `equals`
- foreach 循环里禁止进行元素的 `remove`/`add` 操作，应使用 `Iterator`
- `Arrays.asList()` 返回的集合不能使用 `add`/`remove`/`clear` 方法
- 集合转数组必须使用 `toArray(T[] array)`，避免类型转换异常
- 泛型通配符：`<? extends T>` 上界通配符不能 `add`，`<? super T>` 下界通配符不能 `get`
- 不要在 `equals` 方法中依赖 `hashCode` 的结果

## 代码质量强制要求

- 禁止空指针：所有可能为 null 的返回值必须判空或使用 `Optional`，禁止信任外部输入
- 禁止魔法值：代码中不允许出现未解释的硬编码常量，必须定义为命名常量或枚举
  - 禁止：`if (status == 1)`
  - 正确：`if (status == UserStatusEnum.ACTIVE.getCode())`
- 禁止 `==` 比较对象值，字符串使用 `equals()`，对象使用 `Objects.equals()`
- 集合操作前必须判空，使用 `CollectionUtils.isEmpty()`
- 数值计算注意溢出和精度，金额必须使用 `BigDecimal`，禁止使用 `float`/`double`
- 日期时间使用 `LocalDateTime`，禁止使用 `java.util.Date`
- 禁止在循环中拼接字符串，使用 `StringBuilder`
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

## 构建工具

### Maven
- 使用 `pom.xml` 管理依赖
- 常用命令：`mvn clean install`、`mvn test`、`mvn package`
- 通过 properties 统一管理版本号

## 测试规范

- 使用 JUnit 5 进行单元测试
- 使用 Mockito 进行模拟
- 使用 AssertJ 进行流式断言
- 测试命名：`should_{期望行为}_when_{条件}`
- 重点覆盖 Service 层，确保高测试覆盖率

## 异常处理

- 自定义异常继承 RuntimeException，错误信息必须使用中文
- 建立异常层次结构：`BaseException -> NotFoundException, ValidationException, BusinessException`
- 异常类必须包含错误码（code）和中文错误信息（message）
- AutoCloseable 资源使用 try-with-resources
- 捕获异常时禁止空 catch 块，至少记录日志
- 禁止使用 `e.printStackTrace()`，必须通过日志框架记录异常
- 返回类型为基本数据类型时，return 包装数据类型对象可能自动拆箱产生 NPE，必须判空
- 集合元素即使 `isNotEmpty`，取出的数据元素也可能为 null，使用前必须判空

## 日志规范

- 使用 SLF4J + Logback，禁止使用 `System.out.println()`
- 日志信息必须使用中文，参数化格式：`log.info("用户 {} 已登录", userId)`
- 绝不记录敏感数据（密码、令牌、身份证号、银行卡号）
- 日志级别使用规范：
  - ERROR：系统异常、不可恢复错误，必须附带完整异常栈
  - WARN：潜在问题、业务异常、降级处理
  - INFO：关键业务节点
  - DEBUG：调试信息，生产环境关闭
- 异常日志必须包含上下文信息：`log.error("操作失败，参数：{}", param, e)`
- 生产环境禁止使用 DEBUG 级别日志输出

## 最佳实践

- 常用设计模式：单例（枚举实现）、建造者模式、Record（Java 16+）
- 使用 `var` 进行局部变量类型推断（Java 10+），但不得降低可读性
- 使用 `sealed class` 限制继承层次（Java 17+）
- 使用 `record` 定义不可变数据载体（Java 16+）
