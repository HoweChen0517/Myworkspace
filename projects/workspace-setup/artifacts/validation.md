# 初始化验证

Date: 2026-09-05
Reviewer: Codex
Scope: SPEC 中定义的本地 Markdown Workspace MVP。

## 完成条件
协议与薄适配器可读取，推荐目录与必要模板齐备，模板可建立实例，导航链接有效，Git 隔离生效，交接入口包含事实、决策和下一步。

## 检查记录
通过 Python 标准库读取 Markdown、解析本地链接，使用临时目录复制需求模板并核对实例；通过 Git CLI 检查仓库与忽略规则。

| 检查 | 结果 | 实际证据 |
| --- | --- | --- |
| 必要文件 | PASS | 核对 21 个入口、标准与必需需求模板文件，均存在 |
| Markdown 导航 | PASS | 58 个 Markdown 文件中的 93 个本地链接均指向现有路径 |
| 工作流契约 | PASS | 6 个工作流均具有 Trigger、Input、Steps、Required Context、Output、Validation、Persistence |
| 恢复结构 | PASS | 通用模板、项目模板、需求模板及初始化项目的 status / handoff 必需章节齐全 |
| 模板实例化 | PASS | 在临时目录建立 SIMPLE、STANDARD、COMPLEX 三类需求，最小阶段文件和实例链接通过检查 |
| Git 仓库 | PASS | git init 成功，git status --short 可列出待跟踪产物 |
| 私有资料及缓存隔离 | PASS | git check-ignore 验证 company-private、.env、.venv、node_modules、tmp 共 5 个路径被忽略 |
| 公共文件可跟踪 | PASS | WORKSPACE、portable 索引、PRD 模板共 3 个路径保持可跟踪 |
| 内容检查 | PASS | 活跃产物占位项检查通过；命名标准中的占位符语法作为文档示例核对；生成文件行尾空白检查通过 |
| 来源保留 | PASS | SPEC.md 保留于根目录，初始化产物引用原始规格 |

git diff --check 执行成功；当前文件尚未跟踪，新增文件的空白检查由逐文件扫描覆盖。首次检查识别到命名标准用于说明模板语法的字面占位符，经核对属于文档示例，检查范围据此修正。

## 恢复能力核对

| SPEC 场景 | 本地证据 | 实际场景状态 |
| --- | --- | --- |
| A 换会话 | 项目 README 可进入 status、handoff、context、decisions | UNKNOWN：待独立会话恢复 |
| B 换 Agent | 两个薄适配器均引用 WORKSPACE；交接文件包含下一步 | UNKNOWN：待另一 Agent 接续 |
| C 换设备 | Git 已初始化，同步步骤与私有资料依赖已记录 | UNKNOWN：待批准远程、首次提交和第二设备实测 |
| D 长需求 | 胶囊具备状态、交接、决策、产物与变更模板 | UNKNOWN：待实际长期需求验证 |
| E 并行 Agent | Task 模板定义负责人、输入版本、输出、完成条件和整合交接 | UNKNOWN：待实际并行任务与整合验证 |

## 结论
本地 MVP 初始化完成条件已满足。业务评审、跨设备恢复和长期使用效果按对应实际场景继续验证。

## 外部验证范围
真实跨 Agent 会话、跨设备同步、两周需求实践与多 Agent 并行整合需要在对应环境执行，后续按 SPEC 场景 A–E 记录证据。
