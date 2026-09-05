# 命名与元数据

| 对象 | 格式 |
| --- | --- |
| 项目 | `projects/lower-kebab-case/` |
| 需求 | `REQ-YYYYMMDD-short-name` |
| 任务 | `TASK-001-short-name.md`，编号在需求内唯一 |
| 决策 | `D-001`，编号在对象内唯一 |
| 需求条目 / 验收条件 | `FR-001` / `AC-001` |
| 周报 | `weekly-YYYY-MM-DD.md` |

Markdown 使用 UTF-8，文件名采用稳定标识，正文可用中文。日期使用 ISO `YYYY-MM-DD`；需要时间时包含时区。路径链接相对当前文件，跨私有仓库引用使用批准的位置与资料编号。

对象至少记录 ID 或名称、Owner、Status、Last Updated。未知值填写 `Unknown`；不适用项填写 `N/A` 并说明原因。模板中的 `{{...}}` 在实例化时填写。
