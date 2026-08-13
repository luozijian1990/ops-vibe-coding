# Kubernetes Deployment Restart Golden Path

> Full Track documentation example. It shows how artifacts pass confirmed facts
> and constraints between stages. It is not a running Kubernetes platform or
> production test evidence.

## Problem

内部 Kubernetes Operations Platform 的用户需要安全地 Restart 一个
Deployment，并确认执行结果和审计记录，而不是依赖人工执行 `kubectl`。

## Why Full Track

Restart 会修改 Kubernetes 资源，并涉及授权、确认、凭证范围和审计，因此本案例
按照 [Full Track](../../workflows/full.md) 展示，不属于 Lightweight Track。

## Golden Path

```text
Prototype
→ React Mock
→ Parity
→ Architecture Draft
→ Grill
→ Architecture Final
→ Spec
→ Vertical Tickets
→ TDD Evidence
→ Code Review
→ Ops Readiness
→ E2E
→ Human Acceptance
```

## Artifact Chain

| 阶段 | Artifact | 提供给下一阶段的内容 |
|---|---|---|
| Prototype Review | [approval.md](01-prototype/approval.md) | 已批准的 Workflow、范围和非目标 |
| React + Mock | [contract.md](02-react-mock/contract.md) | 可执行页面、交互、状态和 Mock API 契约 |
| Parity | [prototype-parity.md](03-parity/prototype-parity.md) | React Mock 是否保留已批准产品意图 |
| Architecture Draft | [stack-selection.draft.md](04-architecture/stack-selection.draft.md) | UI 暴露的能力候选、未知约束和最小候选架构 |
| Grill | [grill-decisions.md](04-architecture/grill-decisions.md) | 由人确认的部署、安全和可靠性约束 |
| Architecture Final | [stack-selection.md](04-architecture/stack-selection.md) | 由产品能力与运行约束共同支持的最小架构 |
| Spec | [deployment-restart.spec.md](05-spec/deployment-restart.spec.md) | 可测试的行为、授权、API、审计和失败契约 |
| Tickets | [ticket-01.md](06-tickets/ticket-01.md)、[ticket-02.md](06-tickets/ticket-02.md) | 按用户价值拆分的纵向实现边界 |
| TDD | [tdd-evidence.md](07-implementation/tdd-evidence.md) | RED、GREEN、REFACTOR、VERIFY 应留下的证据形态 |
| Review | [code-review.md](08-review/code-review.md)、[ops-readiness.md](08-review/ops-readiness.md) | 实现符合性和可接受的运行风险 |
| E2E | [test-report.md](09-e2e/test-report.md) | 完整用户 Workflow 的示例测试记录 |
| Human Acceptance | [human-acceptance.md](10-acceptance/human-acceptance.md) | 人工最终判断应消费的输入和记录格式 |

完整产物定义以 [Artifact Map](../../references/artifacts.md) 为准。本目录只展示一条
Artifact → Artifact 的具体链路，不重新定义 Workflow 或 Gate 状态。

## Evidence Boundary

本目录中的 `PASS`、`APPROVED`、命令和测试结果都是教学用 illustrative records，
不是对真实系统执行后的证据。真实项目必须替换示例版本、环境、命令输出、截图或日志，
并由有权限的人完成风险接受和最终验收。
