# Full Track

适用于团队工具、长期维护、生产使用、敏感数据或基础设施变更。每一阶段按 Why、Input、Action、Output、Gate、Next 执行。

## 1. Explore — HTML Prototype

- **Why**：以最低成本把模糊想法变成可体验产品。
- **Input**：Ops Idea、用户和业务痛点。
- **Action**：选择或制作 HTML，覆盖导航、关键流程和重要状态。
- **Output**：Prototype 与初始流程说明。
- **Gate**：无；允许快速探索。
- **Next**：Human Prototype Review。

## 2. Human Prototype Review

- **Why**：在正式工程化前确认产品意图。
- **Input**：可交互 Prototype。
- **Action**：真实使用者检查信息架构、操作入口、状态反馈、权限表现与响应式行为。
- **Output**：批准的 Prototype、关键流程和 viewport 契约。
- **Gate**：`Prototype Approved`。
- **Next**：React + Mock。

## 3. React + Mock

- **Why**：把批准的视觉与交互变成可运行产品契约。
- **Input**：Prototype 与批准记录。
- **Action**：默认保真转换；实现页面、路由、数据模型、API Shape、交互和 Loading/Error/Empty/Success。
- **Output**：React App、Mock API/Data、build 与浏览器验证记录。
- **Gate**：关键流程实际可运行；未验证标为 `NOT_VERIFIED`。
- **Next**：Prototype Parity Gate。

## 4. Prototype Parity Gate

- **Why**：防止工程化转换改变已批准设计。
- **Input**：批准的 HTML、React App、关键流程。
- **Action**：在真实浏览器比较 Visual Parity 和 Interaction Parity。
- **Output**：`PASS`、`PASS_WITH_NOTES` 或 `FAIL` 报告。
- **Gate**：`PASS`，或人接受 `PASS_WITH_NOTES`；`FAIL` 必须退回修复。
- **Next**：Architecture Draft。

## 5. Architecture Draft

- **Why**：把 UI 信号转成能力候选，而不是直接脑补技术组件。
- **Input**：React Mock 与已知运行约束。
- **Action**：提取 API、数据、交互和外部操作；给出最小暂定架构、排除项、升级条件和 Grill 输入。
- **Output**：`stack-selection.draft.md`。
- **Gate**：Draft 明确写“完成 Grill 后再 Finalize”，不得进入 SDD。
- **Next**：Grill。

## 6. Grill

- **Why**：补齐 UI 无法表达的运行约束。
- **Input**：Draft 的开放问题。
- **Action**：确认部署、规模、网络、认证、权限、审批、审计、凭证、任务时长、重试、恢复、留存和现有基础设施。
- **Output**：可追溯的决策记录或 ADR。
- **Gate**：会改变架构的关键约束已由人确认。
- **Next**：Architecture Final。

## 7. Architecture Final

- **Why**：让组件、部署单元和技术选择同时受产品能力与运行约束支持。
- **Input**：同一 Draft 与 Grill 决策。
- **Action**：映射每条决策，复查组件、部署、安全与可靠性基线、排除项和升级条件。
- **Output**：`stack-selection.md`。
- **Gate**：关键未知项仍存在时继续保持 Draft；不得伪造 Final。
- **Next**：SDD / Spec。

## 8. SDD / Spec

- **Why**：把产品和架构变成精确、可测试的行为契约。
- **Input**：Prototype、Architecture Final、决策记录。
- **Action**：定义功能、边界、API、数据模型、状态机、错误、授权、审计、可观测性、失败行为、测试 seam 和验收条件。
- **Output**：已批准 Spec。
- **Gate**：范围、风险和验收条件由人确认。
- **Next**：Tickets。

## 9. Tickets

- **Why**：把 Spec 变成可独立实现和验证的工作单元。
- **Input**：已批准 Spec。
- **Action**：按用户价值拆纵向切片，声明依赖、非目标和验证方式。
- **Output**：本地 Markdown Ticket 或 GitHub Issue。
- **Gate**：每项能在一个独立上下文中完成并验证。
- **Next**：TDD。

## 10. TDD

- **Why**：用反馈循环约束实现，而不是一次性生成大量代码。
- **Input**：一个 Ticket、必要项目文档和相关代码。
- **Action**：RED → GREEN → REFACTOR → VERIFY；一个 Ticket 使用一个新会话。
- **Output**：实现、测试和可复现验证结果。
- **Gate**：验收测试、单元测试、lint/build 和 Ticket 条件通过。
- **Next**：Code Review。

## 11. Code Review

- **Why**：分别检查仓库规范与 Spec 符合度。
- **Input**：固定比较点、Spec、代码和测试。
- **Action**：沿 Standards 与 Spec 两条轴审查。
- **Output**：带文件证据和严重级别的报告。
- **Gate**：阻断问题解决。
- **Next**：Ops Readiness Review。

## 12. Ops Readiness Review

- **Why**：判断运维软件是否具备合理的生产运行和操作安全基础。
- **Input**：架构、Spec、代码、部署配置和测试证据。
- **Action**：按风险检查危险操作、授权、审计、Secret、远程执行、Kubernetes、异步任务、API、可观测性、数据生命周期、部署和恢复。
- **Output**：`PASS`、`PASS_WITH_RISKS` 或 `FAIL` 报告。
- **Gate**：无未修复且未获授权接受的主要风险；关键控制有足够证据。
- **Next**：E2E Test Plan。

## 13. E2E Test Plan → Playwright → Test Report

- **Why**：以真实用户 Workflow 验证系统，而不是只看元素存在。
- **Input**：Spec、Tickets、运行环境和允许操作边界。
- **Action**：先生成测试任务并等待人批准，再运行 Playwright；失败先分类，修复需要额外授权。
- **Output**：步骤、证据、`PASS / FAIL / SKIPPED` 和环境限制。
- **Gate**：破坏性或生产操作有明确权限；测试范围和结果由人确认。
- **Next**：Human Acceptance。

## 14. Human Acceptance

- **Why**：AI 的测试结论不能代替业务和风险判断。
- **Input**：全部产物、审查和测试证据。
- **Action**：人确认产品意图、价值、失败行为、风险与运行成本。
- **Output**：接受、补充或退回。
- **Gate**：只有人可以关闭最终闭环。

## 回退规则

| 失败 | 回退位置 |
|---|---|
| 原型流程不合理 | Prototype / Human Review |
| React 改变批准设计 | React + Mock，修复后重跑 Parity |
| 运行约束不足 | Grill，再 Finalize |
| Spec 无法测试 | Spec 的 seam 与验收条件 |
| Ticket 过大 | 重新拆纵向切片 |
| Review 发现设计问题 | 对应 Spec 或 Architecture 决策 |
| Ops Readiness 失败 | 有边界的修复 Ticket，完成后重审 |
| E2E 失败 | 先分环境、测试、产品、数据或权限问题再行动 |
