# SQL 分析

## Trigger
数据查询、指标口径分析或表关系核验。

## Input
业务问题、指标定义、数据库方言与经批准的数据来源。

## Required Context
相关 database / business 条目及 [SQL 标准](../../standards/sql.md)。

## Steps
1. 确认时间范围、时区、粒度、过滤、去重、空值和权限。
2. 根据可核验 Schema 编写 SQL，说明 JOIN 基数与数据来源。
3. 有可用授权环境时以只读方式执行限量查询，核对行数、重复、缺失和聚合结果。
4. 记录执行环境、时间、参数与结果；执行依赖待具备时标记 UNKNOWN。

## Output
项目 artifacts 或需求目录中的 SQL 和分析说明。

## Validation
核对方言、字段存在性、JOIN 放大、分母、时间边界、样本与总量。静态检查和实际执行分别记录。

## Persistence
保存 SQL、结果证据和局限；按公共会话结束协议更新状态，复用口径进入知识索引。
