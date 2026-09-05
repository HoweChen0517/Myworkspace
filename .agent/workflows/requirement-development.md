# 需求开发

## Trigger
新建需求或继续需求分析、设计与 PRD。

## Input
当前需求 brief、status、handoff、已登记的来源材料。

## Required Context
按需加载 context、有效 decisions、相关知识条目，以及 [需求分析](../../standards/requirement-analysis.md)、[PRD](../../standards/prd.md)、[验证](../../standards/validation.md)。

## Steps
1. INTAKE：确认问题、目标、范围、角色、触发、现状、约束和依赖，登记复杂度、依据与 Done Criteria。缺失事实写入 Open Questions。
2. ANALYSIS：STANDARD / COMPLEX 分析用户旅程、系统边界、规则、数据依赖、异常、状态、权限和兼容性，写入 analysis。
3. DESIGN：STANDARD / COMPLEX 按需设计页面、字段、数据流、接口、状态机、后台任务和错误处理，写入 design；方案决策进入 decisions。
4. PRD：依据 brief 与已确认分析、设计、决策编写 prd，逐项链接验收条件。
5. VALIDATION：独立复核范围、主流程、异常、边界、状态、字段、来源、映射、权限、兼容和验收标准，记录证据与问题。
6. HANDOFF：整理结论、未决项和下一步。COMPLEX 按阶段增加 research、review、实施任务跟踪、acceptance 和 maintenance 记录。

## Output
按复杂度形成 brief、analysis、design、prd、validation 及必要阶段产物。

## Validation
每条需求与验收条件可追溯；每项检查标记 PASS / FAIL / UNKNOWN / N/A 并附证据或理由。完成条件依据实际阶段判断。

## Persistence
执行公共会话结束协议，更新当前需求 status、handoff、相关 decisions 与项目需求索引。
