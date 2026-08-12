# Task and Session Lifecycle

> One Ticket. One Primary Goal. One Clear Stop Condition.

## 这一篇解决什么问题

AI 会话最容易失控的方式：一个会话同时做多个任务、任务目标模糊、没有停止条件、做一半就"差不多完成"了。Task Lifecycle 把每个任务定义成有起点、有验收、有交接记录的小闭环。

## 任务生命周期

```text
Read Ticket
    ↓
Load Required Context
    ↓
Confirm Acceptance Criteria
    ↓
Implement
    ↓
Verify
    ↓
Record Evidence
    ↓
Close / Handoff
```

## 核心原则

```text
One Ticket
One Primary Goal
One Clear Stop Condition
```

一个会话只做一个 Ticket，只有一个主要目标，并且有明确可核对的停止条件（测试通过、验收逐条满足）。不要一个 Session 同时做认证、数据库、前端重构、部署、测试框架。

## 怎么执行

1. **Read Ticket**：先读 Ticket，确认"要交付什么、怎么算完成"；
2. **Load Required Context**：按 [Context Engineering](01-context-engineering.md) 只加载当前任务需要的文档；
3. **Confirm Acceptance Criteria**：开工前先确认验收条件，不让 Agent 自己定义完成标准；
4. **Implement + Verify**：小步实现、立即验证（见 [TDD and Feedback Loop](../docs/05-tdd-and-feedback-loop.md) 的 RED → GREEN → REFACTOR → VERIFY）；
5. **Record Evidence**：把验证结果写进产物，而不是只在对话里说"完成了"；
6. **Close / Handoff**：完成后关闭 Ticket，或写 Session Handoff 交接。

## 停止条件（Stop Condition）

停止条件必须可核对，例如：

```text
- 验收测试通过
- 单元测试通过
- lint / build 通过
- Ticket 验收条件逐条满足
```

"差不多了"不是停止条件。

## Session Handoff

长任务跨会话时，新会话必须能凭文档恢复，不能依赖上一段聊天记录。Handoff 至少包含：

```text
Goal           本次任务目标
Completed      已完成
Verification   验证结果
Remaining      未完成
Blockers       阻塞
Decisions      关键决策
Next Step      下一步
```

示例见 [examples/handoff.example.md](examples/handoff.example.md)。

## 常见错误（Anti-patterns）

- **一个会话做多个 Ticket**：上下文污染，验收证据串台；
- **任务没有明确目标**：Agent 自己发挥"完善一下"，范围无限膨胀；
- **没有停止条件就收尾**：验收条件没人对过，靠感觉宣布完成；
- **做一半换会话不留 Handoff**：新会话从零摸索，重复踩坑。

## 门禁与下一步

- **门禁**：Ticket 验收条件逐条满足，验证证据可复现；
- **下一步**：[04-gates-and-evidence.md](04-gates-and-evidence.md)，让门禁留下可检查的工程制品。
