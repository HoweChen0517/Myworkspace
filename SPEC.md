# SPEC.md

## 1. 项目名称

Agent-native Product Development Workspace

简称：

`MyWorkspace`

---

## 2. 项目目标

构建一套面向产品经理日常工作的、本地持久化、跨设备、跨 Agent 的 AI 工作区。

该工作区需要解决以下问题：

1. 工作主要发生在公司工作机，但需要能够在其他设备继续工作。
2. 使用的 Agent 不固定，可能包括：
   - Claude Code
   - OpenAI Codex
   - ChatGPT
   - 飞书中的豆包或其他办公 Agent
3. Agent 自身的 Conversation、Memory、Context 不具备稳定的跨工具继承能力。
4. 长需求可能持续数天或数周，无法依赖单一会话维持完整上下文。
5. 产品工作涉及 PRD、会议纪要、接口、SQL、数据库结构、业务规则、需求决策等大量异构信息。
6. 不希望每次更换设备或 Agent 时重新解释项目背景和当前进度。

因此，本系统需要将工作状态从 Agent 内部迁移到外部 Workspace。

核心设计原则：

> Workspace 是工作状态的唯一可信来源，Agent 只是 Workspace 的读取者和修改者。

系统不以“同步聊天记录”为主要手段，而通过结构化 Artifact 实现工作连续性。

---

# 3. 核心设计原则

## 3.1 Artifact-driven，而非 Conversation-driven

所有需要长期保留的信息必须进入 Workspace。

聊天内容本身不视为正式工作状态。

任何以下内容如果仅存在于 Chat 中，都应认为尚未持久化：

- 需求结论
- 业务规则
- 技术方案
- 字段定义
- API 映射
- 需求范围
- 关键判断
- 待办事项
- 风险
- 决策原因

Agent 在完成重要工作后必须将结果写回 Workspace。

---

## 3.2 Agent 可替换

Workspace 不绑定具体 Agent。

Claude Code、Codex、ChatGPT 或其他 Agent 应能够基于同一套 Workspace Protocol 恢复工作状态。

目标：

```text
Claude Code
     │
Codex
     ├──→ Workspace
ChatGPT
     │
Feishu Agent
```

Agent Adapter 应尽可能薄。

公共规范只能维护一套。

禁止长期形成：

```text
Claude 专属规则
Codex 专属规则
ChatGPT 专属规则
```

导致规范分叉。

推荐：

```text
Canonical Workspace Protocol
          ↓
     Thin Adapters
```

---

## 3.3 可恢复，而不是完整记忆

系统不要求新 Agent 获得上一 Agent 的完整推理过程。

系统只要求新 Agent 可以回答：

1. 当前项目是什么？
2. 当前需求是什么？
3. 现在进行到哪？
4. 已经确认了什么？
5. 为什么这么决定？
6. 还有什么没解决？
7. 下一步做什么？
8. 需要读取哪些资料？

因此核心恢复文件为：

```text
status.md
handoff.md
context.md
decisions.md
```

---

## 3.4 渐进式上下文加载

禁止默认将完整知识库、完整项目历史、完整需求文档全部放入一次 Agent Context。

Agent 应：

```text
Task
↓
读取当前状态
↓
判断任务类型
↓
读取 Routing Index
↓
加载相关 Knowledge
↓
执行任务
```

目标：

- 减少 Token 消耗
- 降低上下文污染
- 减少旧结论干扰
- 提高长任务稳定性

---

## 3.5 Git 管状态，不依赖 Agent Memory

Git 应承担：

- 跨设备同步
- 历史版本
- Diff
- Snapshot
- Rollback
- 多 Agent 修改追踪

但 Workspace 应保持普通文件结构，不依赖某个 Agent 才能读取。

主要内容优先使用：

- Markdown
- YAML
- JSON
- CSV
- SQL
- Mermaid

避免使用私有数据库格式作为核心信息存储方式。

---

# 4. Workspace 信息模型

系统需要定义以下一级对象：

```text
Workspace
Project
Requirement
Task
Artifact
Decision
Knowledge
Workflow
Status
Handoff
```

---

# 5. 推荐目录结构

Agent 首先按照以下结构构建 Workspace：

```text
myworkspace/
│
├── README.md
├── WORKSPACE.md
├── AGENTS.md
├── CLAUDE.md
├── SPEC.md
│
├── .agent/
│   ├── routing.md
│   │
│   ├── workflows/
│   │   ├── requirement-development.md
│   │   ├── requirement-review.md
│   │   ├── requirement-change.md
│   │   ├── sql-analysis.md
│   │   ├── meeting-processing.md
│   │   └── weekly-report.md
│   │
│   └── skills/
│       └── README.md
│
├── inbox/
│   └── README.md
│
├── projects/
│   └── README.md
│
├── knowledge/
│   ├── index.md
│   │
│   ├── company/
│   ├── systems/
│   ├── business/
│   ├── database/
│   ├── api/
│   └── glossary/
│
├── standards/
│   ├── prd.md
│   ├── requirement-analysis.md
│   ├── api.md
│   ├── sql.md
│   ├── naming.md
│   └── validation.md
│
├── templates/
│   ├── project/
│   ├── requirement/
│   ├── status.md
│   ├── handoff.md
│   ├── context.md
│   ├── decision.md
│   └── task.md
│
├── scripts/
│   └── README.md
│
└── archive/
```

第一阶段不要求所有文件有复杂内容。

优先保证信息架构正确。

---

# 6. Workspace Protocol

`WORKSPACE.md` 是所有 Agent 必须遵循的最高级工作协议。

其职责不是保存业务知识，而是定义 Agent 如何操作 Workspace。

至少包含以下协议。

---

## 6.1 Session Start Protocol

Agent 开始任何非简单任务时：

### Step 1

确认当前 Workspace Root。

### Step 2

读取：

```text
WORKSPACE.md
```

### Step 3

识别当前 Project / Requirement。

### Step 4

读取当前对象的：

```text
status.md
handoff.md
```

如果存在：

```text
context.md
```

根据任务需要读取。

### Step 5

读取：

```text
.agent/routing.md
```

根据任务类型选择需要加载的：

- knowledge
- standard
- workflow

禁止无目的加载整个 Knowledge Base。

---

# 7. Session End Protocol

完成一个有实质性产出的工作 Session 后，Agent 必须检查：

```text
Artifact 是否已更新？
Status 是否已更新？
是否产生新的 Decision？
Handoff 是否需要更新？
是否存在新的 Open Question？
是否存在新的 Blocker？
```

至少更新：

```text
status.md
```

如果形成新的重要认知或结论，则更新：

```text
handoff.md
```

如果形成正式方案决策，则更新：

```text
decisions.md
```

---

# 8. Project Model

每个长期项目建立独立目录，例如：

```text
projects/
└── supply-chain-bill/
    ├── README.md
    ├── status.md
    ├── handoff.md
    ├── context.md
    ├── decisions.md
    │
    ├── requirements/
    ├── research/
    ├── artifacts/
    └── archive/
```

Project 用于保存：

- 项目长期背景
- 项目边界
- 项目涉及系统
- Stakeholder
- 共用业务规则
- 项目长期 Decision
- Requirement Index
- 当前整体状态

不要把单个 Requirement 的细节全部堆入 Project Context。

---

# 9. Requirement Capsule

Requirement 是产品开发 Workspace 中最重要的工作单元。

每个需求建立独立目录：

```text
requirements/
└── REQ-YYYYMMDD-short-name/
    ├── README.md
    ├── brief.md
    ├── status.md
    ├── context.md
    ├── handoff.md
    ├── decisions.md
    ├── analysis.md
    ├── design.md
    ├── prd.md
    ├── validation.md
    ├── changelog.md
    │
    ├── tasks/
    ├── references/
    └── archive/
```

不要求每一个需求都生成全部文件。

Agent 根据 Requirement Complexity 决定。

---

# 10. Requirement Complexity

Agent 在首次处理 Requirement 时，应将需求判断为：

```text
SIMPLE
STANDARD
COMPLEX
```

建议标准：

## SIMPLE

例如：

- 新增一个字段
- 修改一个展示规则
- 简单校验规则
- 文案修改

流程：

```text
brief
→ prd
→ validation
```

---

## STANDARD

例如：

- 新增一个功能
- 接口对接
- 页面交互调整
- 多系统数据联动

流程：

```text
brief
→ analysis
→ design
→ prd
→ validation
```

---

## COMPLEX

例如：

- 跨系统业务流程
- 大型平台能力
- Agent 系统
- 数据平台
- 多模块协同

流程：

```text
brief
→ research
→ analysis
→ design
→ prd
→ review
→ implementation tracking
→ acceptance
→ maintenance
```

禁止为了形式完整而机械生成所有文件。

---

# 11. status.md Specification

`status.md` 用于回答：

> 当前做到哪里了？

推荐模板：

```md
# Status

## Metadata

Project:
Requirement:
Owner:
Status:
Last Updated:

## Current Stage

当前需求阶段。

## Goal

当前工作的直接目标。

## Completed

- 已完成事项

## In Progress

- 当前正在处理事项

## Blocked

- 当前阻塞项

## Open Questions

- 尚未确定的问题

## Next Actions

1. 下一步动作
2. 下一步动作

## Relevant Files

- path/to/file
```

`status.md` 必须保持简洁。

禁止变成完整工作日志。

---

# 12. handoff.md Specification

`handoff.md` 用于回答：

> 如果换一个 Agent，它最需要知道什么？

模板：

```md
# Handoff

## Current Objective

当前正在解决的问题。

## Key Findings

目前已经获得的重要事实。

## Key Decisions

已经形成的重要结论。

## Constraints

任务的重要约束。

## Things Tried

尝试过但未采用的方法。

## Risks

当前风险。

## Open Questions

仍需解决的问题。

## Suggested Next Step

下一 Agent 最适合执行的动作。
```

原则：

Handoff 保存“认知压缩后的结果”。

不要保存完整聊天过程。

---

# 13. decisions.md Specification

采用轻量 ADR 模式。

格式：

```md
# Decisions

## D-001 Decision Title

Status: Proposed / Accepted / Superseded / Rejected
Date:
Owner:

### Context

为什么需要做这个决策。

### Decision

最终采用什么方案。

### Alternatives

考虑过哪些其他方案。

### Reason

选择当前方案的原因。

### Consequences

该方案带来的影响。
```

每个重要设计决策都应形成 Decision Record。

典型场景：

- 接口方案
- 数据模型
- 字段来源
- 系统边界
- 业务规则
- 技术路线
- Agent Workflow

---

# 14. context.md Specification

Context 只保存完成当前需求需要知道的局部事实。

例如：

```md
# Context

## Business Background

## Systems

## Actors

## Existing Process

## Related Requirements

## Known Constraints

## Relevant Knowledge

- knowledge/xxx
```

不要复制 Knowledge Base 内容。

优先通过链接引用。

---

# 15. Knowledge Base

Knowledge 保存可跨 Requirement 复用的事实。

建议分类：

```text
knowledge/

company/
systems/
business/
database/
api/
glossary/
```

例：

```text
knowledge/
├── index.md
│
├── systems/
│   └── erp/
│       ├── overview.md
│       ├── modules.md
│       └── workflow.md
│
├── database/
│   └── erp/
│       ├── tables.md
│       └── relationships.md
│
├── api/
│   └── supply-chain-bill/
│       └── overview.md
│
└── glossary/
    └── finance.md
```

---

# 16. Knowledge Router

`.agent/routing.md` 是 Context Router。

其作用：

根据任务判断应该读取哪些知识。

示例：

```md
# Context Routing

## ERP Database

When task involves:

- database table
- field
- SQL
- table relationship

Read:

knowledge/systems/erp/overview.md
knowledge/database/erp/

Then read:

standards/sql.md

---

## API Integration

When task involves:

- API
- interface
- request
- response
- field mapping

Read:

knowledge/api/
standards/api.md

---

## PRD Writing

When writing or updating PRD:

Read:

standards/prd.md
standards/requirement-analysis.md

---

## Requirement Review

Read:

standards/validation.md
.agent/workflows/requirement-review.md
```

Routing 文件只负责导航。

不要塞入大量业务内容。

---

# 17. Standards Layer

`standards/` 保存工作产物规范。

例如：

```text
prd.md
api.md
sql.md
naming.md
validation.md
```

这些规范回答：

> 怎么做才算符合要求？

例如：

`standards/prd.md`

定义：

- PRD 结构
- 需求描述方式
- 边界条件
- 异常场景
- 数据口径
- 验收标准

Knowledge 与 Standard 必须分开。

Knowledge 回答：

> 事实是什么？

Standard 回答：

> 工作应该怎么做？

---

# 18. Workflow Layer

`.agent/workflows/` 定义 Agent 长任务执行流程。

Workflow 不应是超级 Prompt。

每个 Workflow 定义：

```text
Trigger
Input
Steps
Required Context
Output
Validation
Persistence
```

---

# 19. Requirement Development Workflow

文件：

```text
.agent/workflows/requirement-development.md
```

推荐流程：

```text
INTAKE
↓
ANALYSIS
↓
DESIGN
↓
PRD
↓
VALIDATION
↓
HANDOFF
```

具体定义：

## Phase 1 — Intake

读取：

```text
brief.md
status.md
context.md
```

确认：

- Problem
- Goal
- Scope
- Actor
- Trigger
- Existing Process
- Constraints
- Dependency

信息不足时，将内容写入：

```text
Open Questions
```

禁止自行将未知事实写成确定事实。

---

## Phase 2 — Analysis

分析：

- Actor
- User Journey
- Current Process
- Target Process
- System Boundary
- Business Rule
- Data Dependency
- Exception
- State Transition
- Permission
- Compatibility

输出：

```text
analysis.md
```

---

## Phase 3 — Design

根据需要设计：

- 页面变化
- 字段变化
- 表结构
- API
- 数据流
- 状态机
- 后台任务
- 权限
- Error Handling

输出：

```text
design.md
```

产生重要决策时同步更新：

```text
decisions.md
```

---

## Phase 4 — PRD

读取：

```text
standards/prd.md
```

基于：

```text
brief
analysis
design
decisions
```

生成：

```text
prd.md
```

---

## Phase 5 — Validation

Agent 必须独立执行检查。

检查至少包括：

- Scope 一致性
- 流程完整性
- Happy Path
- Exception Path
- Boundary Condition
- 状态一致性
- 字段一致性
- 数据来源
- API Mapping
- Permission
- Backward Compatibility
- Acceptance Criteria

输出：

```text
validation.md
```

禁止仅输出：

```text
PRD 看起来没有问题。
```

必须列出实际检查结果。

---

# 20. Multi-Agent Collaboration

系统需要允许不同 Agent 处理同一 Requirement 的不同 Task。

例如：

```text
requirements/REQ-xxx/tasks/

TASK-001-api-analysis.md
TASK-002-db-analysis.md
TASK-003-prd-review.md
```

Task 模板：

```md
# Task

## Objective

## Scope

## Input

## Output

## Status

TODO / DOING / BLOCKED / DONE

## Findings

## Decision

## Handoff
```

不同 Agent 可以分别处理不同 Task。

完成后将结论汇总到 Requirement Artifact。

---

# 21. Inbox

`inbox/` 用于接收尚未归档的信息。

典型输入：

- 飞书聊天
- 会议纪要
- Screenshot
- API 文档
- Excel
- 临时想法
- 用户反馈
- 开发反馈

Agent 需要支持：

```text
Inbox
↓
识别所属 Project
↓
识别所属 Requirement
↓
提取事实
↓
更新 Artifact
↓
归档原始材料
```

原始材料和 Agent 提炼后的事实不得混淆。

---

# 22. Agent Adapter

系统维护一个 Canonical Protocol。

Agent-specific 文件仅作为入口。

---

## AGENTS.md

供支持 AGENTS.md 的 Agent 使用。

内容尽量简短：

```md
# Agent Instructions

This repository is an Agent-native Product Development Workspace.

Before starting substantive work:

1. Read WORKSPACE.md.
2. Identify the active Project and Requirement.
3. Read their status.md and handoff.md.
4. Follow .agent/routing.md.
5. Load only relevant context.

Before ending substantive work:

1. Persist outputs.
2. Update status.md.
3. Update handoff.md when necessary.
4. Record significant decisions.
5. Run required validation.
```

---

## CLAUDE.md

不要复制完整 Workspace Protocol。

内容：

```md
# Claude Instructions

Follow the workspace protocol defined in:

WORKSPACE.md

Use:

.agent/routing.md

for context loading and workflow routing.
```

---

# 23. Cross-device Sync

第一阶段使用 Git 作为主要同步机制。

预期工作流：

```text
Device A

git pull
↓
Agent Work
↓
git diff
↓
git commit
↓
git push

Remote Repository

git pull

Device B
```

禁止将 `.venv`、缓存、临时产物等加入版本控制。

Agent 应生成合理 `.gitignore`。

---

# 24. Company Data Boundary

Workspace 必须支持信息隔离。

建议：

```text
knowledge/
├── portable/
└── company-private/
```

或物理拆分 Repository。

Portable 内容例如：

- Prompt
- Workflow
- Template
- 通用方法
- 无敏感信息的个人工作规范

Company-private 内容例如：

- 内部 API
- 数据库结构
- 客户信息
- 内网地址
- 内部业务数据
- 公司文档

具体同步方案必须服从公司安全规则。

Agent 不得默认认为 Company-private 信息可以同步到个人设备或外部 Git。

---

# 25. Feishu Integration Boundary

本地 Coding Agent 可以直接操作 Workspace。

飞书内 Agent 未必能够直接访问本地 Git Workspace。

因此系统不要求所有 Agent 访问完全相同的 Storage。

采用：

```text
Canonical Workspace
+
Agent Projection
```

原则：

```text
Git Workspace
= Agent / Engineering Source of Truth

Feishu
= Collaboration Surface
```

飞书适合保存：

- 对外 PRD
- 会议纪要
- 评审材料
- 团队协作文档

Git Workspace 适合保存：

- status
- handoff
- decisions
- workflow
- skill
- API Mapping
- SQL
- Agent Context
- Markdown Artifact

第一阶段不实现复杂双向同步。

避免出现冲突解决问题。

---

# 26. Validation Philosophy

Agent 不得把自身生成结果视为天然正确。

所有关键 Workflow 至少包含一层 Validation。

复杂工作建议使用：

```text
Generate
↓
Self Review
↓
Artifact Consistency Check
↓
External Evidence Check
```

例如 PRD：

```text
PRD
↓
Requirement Checklist
↓
字段一致性检查
↓
状态流程检查
↓
Acceptance Criteria 检查
```

涉及代码或脚本时：

```text
Code
↓
Run
↓
Test
↓
Verify Output
```

“已完成”必须尽量由 Artifact 或实际验证结果支持。

---

# 27. Status Integrity

Agent 不得因为自己生成了文件，就自动将 Task 标记为完成。

只有满足相应 Done Criteria 才能设置：

```text
DONE
```

例如：

```text
PRD Drafted
```

与：

```text
PRD Reviewed
```

必须区分。

推荐状态：

```text
TODO
DOING
BLOCKED
READY_FOR_REVIEW
DONE
```

---

# 28. MVP Scope

第一阶段不要建设复杂 Agent Platform。

MVP 仅实现：

```text
Git
+
Markdown Workspace
+
Workspace Protocol
+
Project
+
Requirement Capsule
+
status.md
+
handoff.md
+
decisions.md
+
Context Router
+
Requirement Development Workflow
```

第一阶段不实现：

- Agent Orchestrator
- Vector Database
- Automatic RAG
- MCP Server
- 飞书复杂双向同步
- Web UI
- Agent Registry
- 自动多 Agent 调度
- Knowledge Graph

---

# 29. Phase 2

MVP 稳定后再考虑：

- Requirement CLI
- 自动创建 Requirement Capsule
- status 自动汇总
- Markdown Schema Validation
- Git Hooks
- 自动生成 Handoff
- Requirement Index
- 飞书同步
- Attachment Parser
- Agent Skill System

例如未来可以支持：

```bash
workspace new requirement "供应链票据开票结果查询"
```

自动创建：

```text
brief.md
status.md
context.md
handoff.md
decisions.md
```

---

# 30. Phase 3

后续再评估：

- MCP
- Semantic Search
- Local RAG
- Knowledge Graph
- Task Dependency Graph
- 多 Agent Orchestration
- Workspace Dashboard
- Feishu Bot
- 自动会议 → Requirement
- 自动 Requirement → Task
- 自动 Git Snapshot

是否建设这些能力必须基于实际使用痛点决定。

---

# 31. Implementation Requirements for Current Agent

当前 Agent 收到本 SPEC 后，应执行以下任务。

## Step 1

检查当前目录。

如果尚不存在 Workspace，则创建推荐目录。

---

## Step 2

创建：

```text
README.md
WORKSPACE.md
AGENTS.md
CLAUDE.md
.agent/routing.md
```

---

## Step 3

建立：

```text
projects/
knowledge/
standards/
templates/
inbox/
```

---

## Step 4

创建 Requirement Template。

模板至少包括：

```text
brief.md
status.md
context.md
handoff.md
decisions.md
analysis.md
design.md
prd.md
validation.md
```

---

## Step 5

创建：

```text
.agent/workflows/requirement-development.md
.agent/workflows/requirement-review.md
```

---

## Step 6

创建合理：

```text
.gitignore
```

不要覆盖用户现有文件。

---

## Step 7

如果已有 Workspace 内容：

先分析现有结构。

不要直接重构。

输出：

```text
Current Structure
Target Structure
Migration Plan
```

确认可安全迁移后再逐步修改。

---

# 32. Agent Working Rules

执行本 SPEC 时必须遵守：

1. 不过度工程化。
2. MVP 优先。
3. 普通 Markdown 优先。
4. 避免重复信息。
5. Canonical Rule 只能有一份。
6. Agent-specific 配置必须保持 Thin Adapter。
7. Knowledge、Standard、Workflow、Project Context 不得混为一层。
8. 不自动把 Chat 当长期知识。
9. 重要结论必须持久化。
10. 未知信息必须显式标记为 Unknown / Open Question。
11. 不将 Agent 推断伪装成已确认业务事实。
12. 修改现有 Workspace 前先检查已有结构。
13. 所有目录和规则应服务于真实工作，不为框架完整性而增加复杂度。

---

# 33. Architecture Summary

整个系统的最终抽象：

```text
                    Agent
                      │
             Agent Thin Adapter
                      │
                WORKSPACE.md
                      │
             Context / Workflow Router
                 ┌────┴────┐
                 │         │
             Knowledge   Standards
                 │         │
                 └────┬────┘
                      │
                   Project
                      │
                 Requirement
                      │
       ┌──────────────┼──────────────┐
       │              │              │
     Status        Decisions       Artifacts
       │              │              │
       └──────────────┼──────────────┘
                      │
                   Handoff
                      │
                     Git
```

核心循环：

```text
LOAD
↓
UNDERSTAND
↓
WORK
↓
VALIDATE
↓
PERSIST
↓
HANDOFF
```

任何 Agent 都必须围绕这个闭环工作。

---

# 34. Definition of Success

MVP 成功标准不是 Workspace 文件数量，而是以下场景可以顺畅完成。

### Scenario A — 换会话

Claude Code 当前会话结束。

开启新 Claude Code Session 后，只读取 Workspace 即可继续需求。

---

### Scenario B — 换 Agent

Claude Code 完成部分需求分析。

Codex 接手后，不读取 Claude Chat History，仅通过 Workspace 可以继续工作。

---

### Scenario C — 换设备

公司电脑完成工作并同步 Workspace。

另一设备获取 Repository 后可以恢复当前工作状态。

---

### Scenario D — 长需求

一个需求持续两周。

无需保留两周完整聊天上下文，新 Session 仍可以通过：

```text
status
handoff
decisions
artifacts
```

继续推进。

---

### Scenario E — 并行 Agent

Agent A 分析 API。

Agent B 分析数据库。

两者分别输出 Task Artifact。

主 Agent 可以根据 Workspace 汇总结论，而无需访问两个 Agent 的完整 Conversation。

---

如果以上场景可以稳定实现，则 MVP 达标。
