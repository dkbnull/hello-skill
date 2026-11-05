---
name: "sqlserver"
description: "SQL Server开发专家助手。当用户需要进行SQL Server数据库开发、T-SQL编程、SSIS/SSRS、数据库管理或SQL Server性能优化时调用。"
---

# SQL Server 开发技能

你是一位资深 SQL Server 开发工程师。在协助 SQL Server 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 SQL Server 2019+ 版本
- 使用 T-SQL 作为开发语言
- 排序规则：`Chinese_PRC_CI_AS` 或 `Latin1_General_CI_AS`
- 生产环境必须启用完整恢复模式 + 定期备份

> 事务隔离级别跨库口径见 database/skills/sql，跨库项目显式指定。

## 命名规范

- 数据库名：小写 + 下划线分隔（`user_center`、`order_system`）
- 表名：小写 + 下划线分隔，使用复数形式（`users`、`order_items`）
- 字段名：小写 + 下划线分隔（`user_name`、`create_time`）
- 索引名：
  - 主键：`PK_{表名}`（`PK_users`）
  - 唯一索引：`UK_{表名}_{字段名}`（`UK_users_email`）
  - 普通索引：`IX_{表名}_{字段名}`（`IX_users_status`）
  - 聚集索引：`CL_{表名}`
- 存储过程：`usp_{功能描述}`（`usp_get_user_by_id`）
- 函数：`fn_{功能描述}`（`fn_format_date`）
- 视图：`vw_{功能描述}`（`vw_user_orders`）
- 触发器：`trg_{表名}_{操作}`（`trg_users_update`）
- 命名语义化，禁止拼音、无意义缩写

## 表设计规范

- 每个表必须包含以下字段：
  - `id`：BIGINT IDENTITY(1,1)，主键
  - `create_time`：DATETIME2，默认 `GETDATE()`
  - `update_time`：DATETIME2，默认 `GETDATE()`
  - `is_deleted`：BIT，默认 0
- 字段类型选择：
  - 金额：`DECIMAL(p, s)`，禁止使用 FLOAT
  - 日期时间：`DATETIME2`，禁止使用 `DATETIME`
  - 日期：`DATE`
  - 字符串：`NVARCHAR(n)` 支持中文
  - 布尔：`BIT`
  - 唯一标识：`UNIQUEIDENTIFIER`（GUID）
- 每个表必须有主键
- 每个表和字段必须有中文扩展属性说明

## T-SQL 编写规范

- 关键字大写（`SELECT`、`FROM`、`WHERE`）
- 变量名：小写 + 下划线分隔（`@user_name`、`@order_count`）
- 参数名：小写 + 下划线分隔（`@p_user_id`）
- 禁止 `SELECT *`，必须明确指定字段
- 使用 `SET NOCOUNT ON` 减少网络流量
- 使用 `TRY...CATCH` 处理错误
- 分页使用 `OFFSET...FETCH`，禁止 `ROW_NUMBER()` 深度分页
- 临时表使用 `#` 前缀（`#temp_users`），表变量使用 `@` 前缀

## 索引规范

- 单表索引数不超过 8 个
- 聚集索引选择：
  - 优先使用自增 ID
  - 禁止在 GUID 上建聚集索引（导致页分裂）
- 非聚集索引：
  - 高频查询条件字段
  - 外键字段
  - 排序字段
- 使用 `INCLUDE` 包含查询需要的非键列
- 使用 `EXEC sp_helpindex` 查看索引信息
- 使用 `SET STATISTICS IO ON` + `SET STATISTICS TIME ON` 分析查询

## 注释规范

- 每个存储过程/函数顶部必须有中文注释说明用途
- 参数必须注释说明含义
- 复杂逻辑必须添加中文行内注释
- 表和字段必须使用扩展属性添加中文说明
- 禁止无意义注释

## 代码质量强制要求

- 通用 SQL 规范（禁止 `SELECT *`、更新/删除带 WHERE、分批执行、逻辑删除、禁止循环 SQL 等）遵循 database/skills/sql 技能

## 性能优化

- 使用执行计划分析查询（`SET SHOWPLAN_TEXT ON`）
- 关注：Table Scan、Clustered Index Scan、Key Lookup
- 优化器提示谨慎使用
- 统计信息自动更新保持开启
- 临时表 vs 表变量：大数据量使用临时表（支持索引）
- 使用 `WITH(NOLOCK)` 减少读锁（允许脏读场景）
- 分区表：大表按时间或范围分区

## 监控与诊断

- DMV 查询统计：`sys.dm_exec_query_stats` 按总耗时 / CPU / 逻辑读排序定位 Top SQL，配合 `sys.dm_exec_sql_text`、`sys.dm_exec_query_plan` 关联取出 SQL 文本与执行计划
- 实时会话排查：`sys.dm_exec_requests` 查看当前请求与阻塞链（`blocking_session_id`），`sys.dm_os_waiting_tasks` 定位资源等待的具体任务
- Extended Events：替代已弃用的 SQL Trace / Profiler，采集死锁（`xml_deadlock_report`）、阻塞超阈值、登录失败等事件；会话只抓目标事件并限制内存与落盘，避免生产开销
- Query Store：按库启用（`ALTER DATABASE ... SET QUERY_STORE = ON`），保留查询计划历史与运行时统计，定位计划回退（参数嗅探），必要时使用 `sp_query_store_force_plan` 强制执行计划
- 等待统计：采样分析 `sys.dm_os_wait_stats`（可用 `DBCC SQLPERF('sys.dm_os_wait_stats', CLEAR)` 清零重计），按等待类型归因：`PAGEIOLATCH_*` → IO 瓶颈、`LCK_M_*` → 锁争用、`WRITELOG` → 事务日志写入瓶颈、`CXPACKET` → 并行度
- 基线管理：定期留存 DMV / 等待统计快照作为基线，故障时对比基线定位异常增量

## 安全规范

- 最小权限原则
- 参数化查询防止 SQL 注入
- 禁止使用 `sa` 账号运行应用
- 启用审计日志
- 敏感数据加密（Always Encrypted）

## 备份与恢复

- 完整备份：每日
- 差异备份：每 6 小时
- 事务日志备份：每 15 分钟
- 使用 `DBCC CHECKDB` 定期检查数据库完整性

## 最佳实践

- 使用 CTE 提高可读性
- 使用窗口函数替代自连接
- 使用 `MERGE` 实现插入或更新
- 使用 `OUTPUT` 子句获取操作结果
- 使用 `SNAPSHOT` 隔离级别减少锁争用
