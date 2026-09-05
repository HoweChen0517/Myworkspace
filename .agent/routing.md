# Context Routing

先读取当前对象的 status 和 handoff；下表用于选择增量上下文。知识从 [知识索引](../knowledge/index.md) 定位到具体条目。尚未登记的业务资料记录为 Open Question。

| 任务触发 | 按需知识 | 标准 | 工作流 |
| --- | --- | --- | --- |
| 新需求、PRD、功能设计 | business、systems 中相关条目 | [需求分析](../standards/requirement-analysis.md)、[PRD](../standards/prd.md) | [需求开发](workflows/requirement-development.md) |
| API、请求响应、字段映射 | api、systems 中相关条目 | [API](../standards/api.md) | [需求开发](workflows/requirement-development.md) |
| SQL、表字段、数据口径 | database、business 中相关条目 | [SQL](../standards/sql.md) | [SQL 分析](workflows/sql-analysis.md) |
| 需求评审、验收检查 | 当前需求所引用事实 | [验证](../standards/validation.md) | [需求评审](workflows/requirement-review.md) |
| 需求变更、范围调整 | 当前决策及受影响条目 | [PRD](../standards/prd.md)、[验证](../standards/validation.md) | [需求变更](workflows/requirement-change.md) |
| 会议、聊天、反馈整理 | 涉及项目与来源材料 | [需求分析](../standards/requirement-analysis.md) | [会议处理](workflows/meeting-processing.md) |
| 周报、阶段总结 | 目标周期项目状态与产物 | [验证](../standards/validation.md) | [周报](workflows/weekly-report.md) |

创建对象或调整索引时读取 [命名标准](../standards/naming.md)。跨类型任务按实际需要组合路由。
