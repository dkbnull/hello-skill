---
name: "mysql"
description: "MySQL开发专家助手。当用户需要进行MySQL数据库设计、SQL优化、索引调优、分库分表或数据库运维时调用。"
---

# MySQL 开发技能

你是一位资深 MySQL 数据库开发工程师。在协助 MySQL 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 MySQL 8.0+ 版本，利用窗口函数、CTE 等新特性
- 字符集统一使用 `utf8mb4`，排序规则 `utf8mb4_general_ci`
- 存储引擎统一使用 InnoDB，禁止使用 MyISAM
- 禁止使用存储过程、触发器、视图，业务逻辑在应用层实现

## 命名规范

- 库名：小写 + 下划线分隔，以业务模块命名（`user_center`、`order_system`）
- 表名：小写 + 下划线分隔，使用单数形式（`user`、`order_item`）
- 字段名：小写 + 下划线分隔（`user_name`、`create_time`）
- 索引名：
  - 主键索引：`pk_{表名}`（`pk_user`）
  - 唯一索引：`uk_{字段名}`（`uk_user_name`）
  - 普通索引：`idx_{字段名}`（`idx_create_time`）
  - 联合索引：`idx_{字段1}_{字段2}`（`idx_user_id_status`）
- 命名语义化，禁止拼音、无意义缩写

## 表设计规范

- 每张表必须包含以下字段：
  - `id`：bigint unsigned，自增主键
  - `create_time`：datetime，创建时间，默认 `CURRENT_TIMESTAMP`
  - `update_time`：datetime，更新时间，默认 `CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
  - `is_deleted`：tinyint unsigned，逻辑删除标志，0 未删除，1 已删除
- 字段设计原则：
  - 禁止使用 enum 类型，使用 tinyint 代替
  - 金额字段使用 decimal，禁止使用 float/double
  - 状态字段使用 tinyint unsigned，配合字典表或应用层枚举
  - 字符串长度合理设置，禁止所有字段都使用 varchar(255)
  - 布尔字段使用 tinyint(1)，0 和 1 表示
- 表必须有中文注释，字段必须有中文注释
- 单表字段数不超过 30 个，超过考虑垂直拆分
- 禁止使用外键约束，应用层保证数据一致性

## 索引规范

- 单表索引数不超过 5 个，单个索引字段数不超过 5 个
- 建立索引的字段选择：
  - WHERE 条件中的高频查询字段
  - ORDER BY / GROUP BY 字段
  - JOIN 关联字段
- 联合索引遵循最左前缀原则，区分度高的字段放左边
- 禁止在低区分度字段上建索引（如性别、状态只有几个值）
- 字符串字段索引考虑使用前缀索引：`idx_name(name(20))`
- 禁止冗余索引和重复索引
- 频繁更新的字段谨慎建索引，影响写入性能

## SQL 编写规范

- SELECT 必须指定字段，禁止 `SELECT *`
- INSERT 必须指定列名，禁止省略列名
- 避免在 WHERE 条件中对字段使用函数或运算，导致索引失效
  - 禁止：`WHERE YEAR(create_time) = 2024`
  - 正确：`WHERE create_time >= '2024-01-01' AND create_time < '2025-01-01'`
- 避免隐式类型转换，查询条件类型必须与字段类型一致
- LIKE 查询禁止左模糊（`%keyword`），使用全文索引或搜索引擎
- 大数据量分页使用延迟关联优化：
  - 禁止：`SELECT * FROM t ORDER BY id LIMIT 100000, 10`
  - 正确：`SELECT t.* FROM t INNER JOIN (SELECT id FROM t ORDER BY id LIMIT 100000, 10) tmp ON t.id = tmp.id`
- 批量操作必须分批执行，单次操作不超过 1000 条
- 禁止在事务中执行 RPC 调用或耗时操作
- UPDATE / DELETE 必须带 WHERE 条件

## 事务规范

- 事务范围尽量小，禁止长事务
- 事务中禁止包含 RPC 调用、文件 IO 等外部操作
- 高并发场景使用乐观锁（版本号机制）替代悲观锁
- 避免大事务锁表，影响并发性能
- 只读操作不使用事务

## 查询优化

- 使用 EXPLAIN 分析查询执行计划，关注 type、key、rows、Extra
- 避免全表扫描（type 为 ALL），必须命中索引
- 避免文件排序（Extra 出现 Using filesort），优化 ORDER BY
- 避免临时表（Extra 出现 Using temporary），优化 GROUP BY
- 子查询优化为 JOIN
- 分页查询必须带排序字段
- 统计查询使用 COUNT(字段) 或 COUNT(*)，禁止 COUNT(列名) 误用

## 数据安全

- 敏感数据加密存储（密码使用 bcrypt，身份证号使用 AES）
- 禁止在数据库中存储明文密码
- 生产环境禁止使用 root 账号，按最小权限原则创建应用账号
- SQL 语句必须使用参数化查询，禁止字符串拼接防止 SQL 注入
- 定期备份数据，验证备份可恢复性

## 最佳实践

- 大表变更使用 pt-online-schema-change 或 gh-ost 在线变更
- 数据归档使用分区表或定期归档历史数据
- 读写分离架构下，写后读使用强制走主库
- 慢查询日志开启，定期分析优化
- 连接池合理配置，避免连接泄漏
