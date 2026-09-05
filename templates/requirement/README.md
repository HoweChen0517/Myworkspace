# {{requirement-title}}

Requirement ID: {{REQ-YYYYMMDD-short-name}}
Project: {{project-slug}}
Owner: {{owner}}
Complexity: {{SIMPLE / STANDARD / COMPLEX}}
Complexity Reason: {{判断依据}}

## 恢复入口
[Status](status.md) · [Handoff](handoff.md) · [Context](context.md) · [Decisions](decisions.md)

## 产物入口
[Brief](brief.md) · [PRD](prd.md) · [Validation](validation.md)

## 模板选用
在项目 `requirements/` 下创建需求目录，按下表复制本模板文件，并填写占位项。

| 复杂度 | 文件 |
| --- | --- |
| 全部 | README、brief、status、context、handoff、decisions、prd、validation |
| STANDARD | 增加 analysis、design |
| COMPLEX | 增加 analysis、design；按阶段创建 research、review、implementation、acceptance、maintenance |

按实际工作建立 tasks、references、archive；变化需要追踪时使用 changelog。任务从工作区 `templates/task.md` 创建。新增阶段产物时在本文件补充链接，项目 README 同步登记需求。
