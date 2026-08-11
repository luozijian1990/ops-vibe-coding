# Ops Vibe Coding

> Operations-first workflow for turning infrastructure ideas into tested software.

面向 SRE、DevOps、Platform Engineer 和运维开发的 AI 原生软件开发工作流：从可交互原型开始，经过架构确认、规格定义、TDD、运维就绪审查和真实浏览器验收，把运维想法变成可运行、可验证的软件。

```text
Ops Idea
→ HTML Prototype
→ Human Prototype Review
→ React + Mock
→ Prototype Parity Gate
→ Architecture Draft
→ Grill
→ Architecture Final
→ SDD / Spec
→ Tickets
→ TDD
→ Code Review
→ Ops Readiness Review
→ E2E Test Plan
→ Playwright E2E
→ Test Report
→ Human Acceptance
```

## 这是什么

Ops Vibe Coding 不是新的 Agent Framework、Prompt 合集或后台模板。它是一套 `operations-first` 方法：把权限、审计、Secret、失败恢复、部署和可观测性从“上线前补一下”提前到设计与验证过程里。

HTML Prototype 是低成本的可执行需求；React + Mock 是产品契约；Architecture 与 Spec 分别确定系统组成和精确行为；测试、审查和人工验收构成证据闭环。

## 适合谁

- 熟悉运维业务，但不一定具备完整产品或全栈开发经验的人；
- 正在建设 Kubernetes、监控告警、Incident、AIOps 或远程执行工具的团队；
- 希望让 Claude Code、Codex、OpenCode 等 Agent 参与开发，同时保留人工决策门禁的人；
- 需要把 Demo 逐步演进为长期维护或生产使用的软件的人。

## 为什么从 Prototype 开始

“我要做一个 Kubernetes 管理平台”无法回答导航、信息密度、操作流程和失败反馈。一个可交互 HTML 原型能让使用者先体验并确认产品意图，在正式代码成本还很低时发现问题。

原型确认后，工程化转换必须遵守一个原则：

> 组件库服从已经批准的原型，而不是让原型服从组件库。

## 10 分钟理解整个流程

| 阶段 | 回答的问题 | 关键产物 | 人工门禁 |
|---|---|---|---|
| Prototype | 用户如何使用？ | HTML Prototype | 批准页面、流程与交互 |
| React + Mock | 产品契约是否可执行？ | React App、Mock API/Data | 一致性报告通过 |
| Architecture | 系统由什么组成？ | Draft、Grill 决策、Final | 确认运行与风险约束 |
| Spec + Tickets | 系统精确如何工作？ | Spec、纵向切片 Tickets | 批准范围与验收条件 |
| TDD + Review | 实现是否正确？ | 代码、测试、Review | 阻断问题解决 |
| Ops + E2E | 能否安全运行并解决问题？ | Readiness、E2E 报告 | 最终人工验收 |

前端只能暴露能力候选，不能直接决定 Redis、MQ、Worker、WebSocket 或微服务：

> UI 告诉我们系统需要做什么；Grill 告诉我们系统必须在什么约束下运行；两者共同决定架构。

## 从这里开始

- 第一次体验或小工具：阅读 [Quick Start](docs/01-quick-start.md)，走 [Lightweight Track](workflows/lightweight.md)。
- 团队工具、长期维护、生产或敏感操作：直接走 [Full Track](workflows/full.md)。
- 想知道每一步留下什么：查看 [Artifact Map](references/artifacts.md)。
- 想知道用哪些现有能力：查看 [Skill Map](references/skills.md)。

Lightweight Track 会缩短文档与审查深度，但不会把未确认的 Architecture Draft 冒充 Final，也不代表生产就绪。

## 核心原则

- **Prototype before expensive code**：尽量在低成本原型阶段发现产品问题。
- **Human approves intent, AI executes detail**：AI 执行细节，人决定需求、架构、风险和验收。
- **Evidence before complexity**：没有具体证据，不增加基础设施复杂度。
- **One ticket, one context**：一个 Ticket 使用一个新会话，降低上下文污染。
- **Specs are executable contracts**：Spec 必须能够指导实现与测试。
- **Tests are evidence**：不能用“应该可用”代替可复现验证。
- **Operations are part of software design**：权限、审计、恢复和运维成本属于设计本身。
- **Human acceptance closes the loop**：最终由人确认软件是否解决最初的问题。

## 仓库边界

本仓库维护方法论、工作流、教程和示例。可执行 Skill 独立维护在 [`personal-skill`](https://github.com/luozijian1990/personal-skill)；这里引用它们，但不复制其实现。

首版刻意不提供 CLI、Agent Framework、低代码平台或大量 Demo。目标是先让一条 Ops-specific Golden Path 清楚、可执行、可验证。
