# Harness Engineering

> Workflow tells the Agent what comes next. Harness helps the Agent execute that workflow consistently, safely, and with verifiable evidence.

Workflow 告诉 AI 下一步做什么；Harness 负责让 AI 在正确的上下文、规则、权限和验证机制下稳定完成这些工作。

## 解决什么问题

Ops Vibe Coding 的 [Full Track](../workflows/full.md) 已经定义了完整流程：Prototype → Architecture → Spec → Tickets → TDD → Ops Readiness → E2E。但流程定义不等于 Agent 会稳定遵守。在实际执行中：

- 新会话不知道团队规范和项目事实，重新靠人 Prompt；
- 一个会话同时做多个 Ticket，上下文污染，验收证据串台；
- AI 口头说"门禁通过了"，没有任何可追溯的产物；
- AI 能执行危险命令，但没有人确认它是否有权限；
- 换一个 AI Coding 工具，所有积累的规则和知识全部丢失。

Harness 是横跨上述全过程的**工程治理层**，回答四个问题：

```text
AI 应该知道什么？      → Context（上下文）
AI 应该遵守什么？      → Instructions（规则）
AI 怎么证明完成了？    → Gates + Evidence（门禁与证据）
AI 犯错之后如何减少重复犯错？ → Feedback + Refinement（反馈沉淀）
```

## Harness 不是什么

```text
Harness != Workflow        它不是新的开发流程，不定义生命周期
Harness != Skill           它不是具体执行能力，不写代码
Harness != Agent Framework 它不提供运行时、编排器或 Agent 注册表
```

各层的关系：

```text
Workflow
→ 定义生命周期（Prototype → Spec → Ticket → TDD → E2E → Human Acceptance）

Harness
→ 定义执行协议与治理约束（上下文、规则、门禁、权限、反馈）

Skills
→ 提供具体执行能力（tdd、code-review、playwright…）

Claude Code / Codex / OpenCode / Gemini CLI / Cursor
→ 提供执行运行时
```

## 总体模型

```text
                 Harness

 Context ─────────────────┐
 Instructions ────────────┤
 Task State ──────────────┤
 Gates ───────────────────┤
 Evidence ────────────────┤
 Permissions ─────────────┤
 Feedback ────────────────┤
 HITL ────────────────────┤
                          ▼
 Prototype → Spec → Ticket → TDD → E2E
                          │
                          ▼
                  Human Acceptance
```

## 核心原则

- **Context is an engineering asset**：上下文应该被版本化，而不是每次重新 Prompt；
- **Context has scope**：团队、治理、项目和服务知识按作用域继承，不靠人重复 Prompt；
- **AGENTS is an index**：AGENTS.md 是入口，不是知识垃圾场；
- **One ticket, one context**：一个 Ticket 一个上下文，降低 Context Rot 和任务漂移；
- **Gates produce artifacts**：门禁必须留下可检查的证据；
- **Rules have one source of truth**：避免 README、Skill、Workflow 之间逐渐漂移；
- **Capability does not imply permission**：AI 能执行，不代表有权执行；
- **Human controls irreversible decisions**：高风险操作保留人机协同（HITL）；
- **Errors should improve future context**：有复用价值的纠错应沉淀为团队资产；
- **Runtime is replaceable**：工程知识不应该绑定某一个 AI Coding 产品；
- **Contracts should be machine-readable**：接口和跨服务关系尽量由 IDL 与 Service Matrix 表达，AI 不凭猜测补依赖。

## 文档地图

| 文档                                                                        | 解决的问题                                |
| --------------------------------------------------------------------------- | ----------------------------------------- |
| [01-context-engineering.md](01-context-engineering.md)                       | AI 应该知道什么？三层上下文如何版本化     |
| [02-agents-and-instructions.md](02-agents-and-instructions.md)               | AI 应该遵守什么？AGENTS.md 的职责与边界   |
| [03-task-and-session-lifecycle.md](03-task-and-session-lifecycle.md)         | 任务如何开始、执行、验证、交接            |
| [04-gates-and-evidence.md](04-gates-and-evidence.md)                         | 门禁如何留下可追溯的工程制品              |
| [05-feedback-and-self-refinement.md](05-feedback-and-self-refinement.md)     | 纠错如何沉淀，让未来的会话不再犯同样的错  |
| [06-tool-and-permission-boundaries.md](06-tool-and-permission-boundaries.md) | AI 能做什么、有权做什么、什么必须人来批准 |
| [07-runtime-portability.md](07-runtime-portability.md)                       | 换工具时如何保住工程资产                  |

示例：[examples/](examples/) 提供可直接改写的最小模板（AGENTS.md、Context Map、Gate Record、Session Handoff、Service Matrix）。

## 与现有文档的关系

- **Workflow**：[Full Track](../workflows/full.md)、[Lightweight Track](../workflows/lightweight.md) 定义流程与门禁，本目录不重复定义；
- **Context**：[Context Engineering](../docs/04-context-engineering.md) 讲工作流内的上下文分层，本目录将其扩展为可长期维护的治理协议；
- **Skills**：[Skill Map](../references/skills.md) 列出执行能力，本目录规定调用它们的协议与边界。

## 从零开始

1. 阅读 [02-agents-and-instructions.md](02-agents-and-instructions.md)，从 [AGENTS.example.md](examples/AGENTS.example.md) 开始写第一版 AGENTS.md；
2. 按 [01-context-engineering.md](01-context-engineering.md) 建立 `docs/` 知识结构，用 [context-map.example.md](examples/context-map.example.md) 规划每个任务的读取范围；
3. 按 [04-gates-and-evidence.md](04-gates-and-evidence.md) 为现有门禁增加 Gate Record 产物；
4. 按 [06-tool-and-permission-boundaries.md](06-tool-and-permission-boundaries.md) 明确权限边界，高危操作交给人工；
5. 运行几个真实 Ticket，把纠错沉淀进仓库（见 [05-feedback-and-self-refinement.md](05-feedback-and-self-refinement.md)）。
