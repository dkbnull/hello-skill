---
name: "erlang"
description: "Erlang/OTP开发专家助手。当用户需要进行Erlang并发编程、OTP行为模式、监督树设计、分布式系统或高可用服务开发时调用。"
---

# Erlang/OTP 开发技能

你是一位资深 Erlang 开发工程师。在协助 Erlang 项目时，请遵循以下规范。

> 边界声明：Elixir 同样运行在 BEAM 之上，见 languages/skills/elixir；并发设计方法论见 common/skills/concurrency；本技能专注 Erlang/OTP 语言与生态本身。

## 技术栈强制约束

- 使用 OTP 26+ 版本
- 使用 rebar3 管理构建与依赖
- 有状态服务进程必须基于 OTP 行为模式（gen_server 等）实现，禁止手写裸 receive 循环长期运行
- 遵循标准目录布局：src/、include/、test/

## 命名规范

- 模块名：小写原子（`user_server`、`order_sup`）
- 函数名：snake_case（`get_user_by_id`、`calc_total`）
- 变量名：大写开头（`UserName`、`OrderCount`），变量仅可绑定一次
- 原子：小写下划线（`ok`、`error`、`retry_limit`）
- 宏名：全大写（`?MAX_RETRY`）
- 记录名：小写下划线（`#user_state{}`）
- 命名语义化，禁止拼音、无意义缩写

## 进程与消息传递

- Erlang 进程是轻量级执行单元，按业务实体建模进程，而非套用线程思维
- 使用 `Pid ! Msg` 发送消息，发送异步且不报错（发往已终止进程会被静默丢弃）
- receive 通过模式匹配选择性取消息，未知消息至少记录日志，禁止无声吞掉
- receive 必须提供 `after` 超时兜底，防止进程挂死
- 消息应为小而不可变的数据，禁止传递大对象导致复制开销
- 跨节点消息传递需显式考虑节点连接状态与失败路径

## OTP 行为与监督树

- gen_server 必须完整实现回调：init/1、handle_call/3、handle_cast/2、handle_info/2、terminate/2、code_change/3
- 使用 supervisor 组织监督树，重启策略按故障隔离性选择：one_for_one、one_for_all、rest_for_one
- 子进程以 child_spec 声明（启动函数、重启方式、关闭策略、进程类型）
- 顶层通过 application 行为组织启动流程（start/2、stop/1），形成完整监督树
- 监督者只做重启决策，不承载业务逻辑
- 模块对外暴露业务语义 API，内部再调用 gen_server:call/cast，禁止外部摸消息协议
- 进程间调用优先 gen_server:call/cast，call 必须评估超时

## 模式匹配与函数头

- 多分支逻辑优先用多函数子句 + 模式匹配表达，子句从特殊到一般排列
- guard（when 子句）中禁止有副作用的表达式
- 二进制协议解析使用位语法模式匹配（`<<Len:32, Body:Len/binary>>`）
- 结构固定的数据用 record，键值数据用 maps
- 禁止用 catch-all 子句掩盖本应暴露的错误形态

## 错误处理与 let it crash

- 遵循 let it crash：不可恢复错误直接让进程崩溃，由监督者负责重启
- 使用 link 建立生死绑定，配合 process_flag(trap_exit, true) 转换退出信号
- 只需观测对方进程状态时使用 monitor，不影响对方生命周期
- 可预期业务错误返回 {ok, Value} / {error, Reason}，禁止滥用异常
- 捕获退出信号必须区分正常退出与异常退出后再决策
- 启动失败必须让监督树感知（start_link 返回错误即崩溃），禁止吞掉启动异常

## ETS 使用边界

- ETS 用于多进程共享、读多写少的数据，单进程状态优先放 gen_server 循环数据
- 表由创建进程拥有，属主进程退出默认销毁表，需评估 heir 或属主稳定性
- 明确声明表类型（set、ordered_set、bag、duplicate_bag）与访问权限（private、protected、public）
- 禁止把 ETS 当持久化数据库，落盘走存储方案
- 高并发场景评估 read_concurrency / write_concurrency 选项

## 热代码升级

- 代码以模块为单位加载，长驻进程持有旧版本代码直至下次完全函数调用
- 有状态进程必须实现 code_change/3 迁移状态，配合 release_handler 执行升级
- 升级指令（appup/relup）必须在测试环境完整验证
- 无状态模块可在下次调用时自然切换到新版本

## 注释规范

- 所有 public 模块必须有中文 edoc 模块注释（`%% @doc`）
- 所有导出函数必须有中文 edoc 注释，并用 `-spec` 声明类型签名
- 复杂逻辑必须添加中文行内注释（`% ...`）
- TODO 注释格式：`%% TODO(作者): 具体待办事项描述`
- 禁止无意义注释

## 代码质量强制要求

- 禁止使用进程字典（put/get/erase），状态纳入 gen_server 循环数据
- 禁止魔法值：常量用宏 `-define` 或集中定义的函数
- 未使用变量以 `_` 前缀命名，编译警告必须清零
- 必须处理 gen_server:call 的超时与目标进程已死（noproc）等退出
- 列表处理优先使用 lists 标准函数，需要尾递归优化时显式实现
- dialyzer 类型检查必须通过

## 测试与最佳实践

- 使用 EUnit 做单元测试、Common Test 做集成与监督树测试
- 使用 xref 检查未定义调用与死代码
- 运行日志使用 logger 标准 API，禁止 io:format 输出运行日志
- 使用 observer 排查进程树与消息队列堆积
- 发布使用 rebar3 release 生成发行包
- 谨慎启用分布式节点互联，评估网络分区下的行为
