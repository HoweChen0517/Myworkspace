# Decisions

## D-001 以 Markdown 与 Git 建立工作区 MVP

Status: Accepted
Date: 2026-09-05
Owner: Workspace 初始化

### Context
[SPEC](../../SPEC.md) 定义跨 Agent 恢复、渐进加载和普通文件存储需求。

### Decision
以 WORKSPACE.md 维护公共协议，AGENTS.md 和 CLAUDE.md 引用入口；项目与需求使用状态、交接、背景和决策文件恢复工作。

### Alternatives
Agent 内部记忆；专用服务与数据库。

### Reason
普通文件便于读取、迁移、版本检查和长期维护，符合 MVP 范围。

### Consequences
状态与索引由每次工作更新，验证和交接成为工作流的固定产物。

## D-002 本地私有目录隔离公司资料

Status: Accepted
Date: 2026-09-05
Owner: Workspace 初始化

### Context
公司资料与通用模板具有不同的同步要求，远程仓库及设备授权范围尚待确认。

### Decision
通用工作区内容使用常规分类；公司资料进入 Git 忽略的 knowledge/company-private 或经批准的独立仓库。

### Alternatives
将全部内容放入同一个可同步目录；立即配置独立远程私有仓库。

### Reason
本地隔离可立即实施，远程位置在批准后配置。

### Consequences
另一设备需要单独取得私有资料。公共产物与索引仍需核对分级，Git 忽略只保护尚未跟踪的路径。
