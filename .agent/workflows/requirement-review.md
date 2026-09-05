# 需求评审

## Trigger
PRD 自检完成、请求评审或阶段验收。

## Input
brief、prd、validation、有效 decisions 和待评审版本。

## Required Context
[验证标准](../../standards/validation.md)、[PRD 标准](../../standards/prd.md)，按需读取设计、接口映射与来源资料。

## Steps
1. 固定评审版本、范围和完成条件。
2. 逐项对照 brief、设计、PRD 与验收条件，检查主流程、异常、边界、状态、字段、来源、API、权限和兼容。
3. 记录问题位置、影响、严重性、证据、负责人和建议修正。
4. 修正后按问题复验；业务评审与自检分别记录人员、时间及结论。

## Output
validation；COMPLEX 可单独维护 review，记录结论与遗留问题。

## Validation
PASS 有证据，N/A 有适用性说明，UNKNOWN 有补证动作；阻塞问题解决且约定评审完成后才能满足相应 Done Criteria。

## Persistence
按公共会话结束协议保存评审结果、状态、交接和新增决策。
