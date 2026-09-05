# MyWorkspace

面向产品工作的本地持久化工作区。项目事实、需求产物、决策和进度以普通文件保存，支持不同 Agent、会话和设备接续工作。

## 开始使用

1. 阅读 [工作协议](WORKSPACE.md)，从 [项目索引](projects/README.md) 选择项目。
2. 阅读项目及当前需求的 `status.md`、`handoff.md`。
3. 使用 [上下文路由](.agent/routing.md) 选择工作流和相关资料。
4. 更新产物并执行验证，将进度和下一步写回状态与交接文件。

当前可恢复入口：[Workspace 初始化](projects/workspace-setup/README.md)。

## 新建项目和需求

- 项目：将 `templates/project/` 复制到 `projects/<project-slug>/`，填写模板并添加到项目索引。
- 需求：在项目的 `requirements/` 下创建 `REQ-YYYYMMDD-short-name/`。按 [需求模板说明](templates/requirement/README.md) 选择文件，填写占位项，再更新项目需求索引。
- 临时输入：存入 [inbox](inbox/README.md)，提取带来源的事实后归入项目。
- 业务知识：从 [知识索引](knowledge/index.md) 登记可复用事实。

## 目录职责

| 目录或文件 | 内容 |
| --- | --- |
| WORKSPACE.md | 唯一公共工作协议 |
| .agent/ | 上下文导航和任务工作流 |
| projects/ | 项目、需求、任务、业务产物和恢复状态 |
| knowledge/ | 可复用事实及来源 |
| standards/ | 产物质量标准 |
| templates/ | 项目和需求起始模板 |
| inbox/ | 待整理材料 |
| archive/ | 工作区级历史材料 |
| scripts/ | 手工操作说明与未来工具入口 |

## Git 与跨设备接续

本地仓库用于差异检查和版本记录。首次保存前检查 `git status --short`、`git diff` 及待加入文件的内容；按文件显式暂存，检查 `git diff --cached` 后提交：

```sh
git add README.md WORKSPACE.md
git diff --cached
git commit -m "docs: establish workspace protocol"
```

配置经批准的远程仓库后，在干净工作树上执行 `git pull --ff-only`，开展工作并提交，随后 `git push`。另一设备获取同一仓库后，从项目状态与交接入口恢复。发生分歧时先核对两边产物、状态和决策，完成合并验证后再同步。

同步范围由公司规则决定。公共模板与方法保存在本仓库；内部 API、数据库结构、客户数据等放入本地 `knowledge/company-private/` 或公司批准的独立仓库。该本地目录由 Git 忽略。Git 忽略规则仅影响尚未跟踪的文件，已跟踪材料需要单独核查。

## 飞书与其他文档 Agent

选择经批准的 PRD、会议纪要或评审材料作为单向导出副本，记录源文件、Git 版本、目标链接、导出时间和负责人。外部反馈经 inbox 整理、核验后写回正式产物。当前范围和扩展方向见 [SPEC](SPEC.md)。
