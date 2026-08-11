# TDD and Feedback Loop

## 这一篇解决什么问题

把 AI 的"做完了"变成可复现的验证结果：测试先行、小步循环、明确停止条件，杜绝"代码应该没问题"这种不可核对的说法。

## 为什么这样设计

一次性生成大量代码的典型结局是：Agent 说"应该没问题"，没有测试、没有 lint、验收条件没人对过。TDD 的价值不在"先写测试"这个动作本身，而在于把每轮开发变成可验证的小闭环：

```text
Goal
 ↓
Implement
 ↓
Verify
 ↓
Feedback
 ↓
Fix / Continue
 ↓
Stop Condition
```

对应原则：**Tests are evidence（测试是证据）**。

## 怎么执行

1. 每个 Ticket 一个会话，读 AGENTS.md → Ticket → 相关 Spec/ADR（见 [Context Engineering](04-context-engineering.md)）。
2. 按 RED → GREEN → REFACTOR → VERIFY 循环：
   - **RED**：先写会失败的验收测试；
   - **GREEN**：最小实现让它通过；
   - **REFACTOR**：清理代码，不改变行为；
   - **VERIFY**：全量测试 + lint/build。
3. 使用明确停止条件，例如：
   - 验收测试通过；
   - 单元测试通过；
   - lint / build 通过；
   - Ticket 验收条件逐条满足。
4. 任一条件不满足就继续修，不允许用自我判断结束。

## 常见错误

- 一次生成几百行代码再"review"：反馈太晚，错误成堆；
- 没有测试就宣布完成：最典型的幻觉式完成；
- 跳过 lint / build：可复现验证不完整；
- 停止条件模糊（"差不多了"）：没有可核对的证据。以 Kubernetes 平台为例，实现 Restart 接口必须带失败用例（集群不可达、资源不存在、权限不足），不能只测 happy path。

## 门禁与下一步

- **门禁**：全部验证通过 + Ticket 验收条件满足；
- **下一步**：Code Review 检查规范与 Spec 符合度，然后进入 [Ops Readiness Review](06-ops-readiness.md)。

## 相关文档

- [Full Track §10~11](../workflows/full.md)：TDD 与 Code Review
- [Skill Map](../references/skills.md)：`tdd`、`code-review`
