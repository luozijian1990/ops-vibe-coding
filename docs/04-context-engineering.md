# Context Engineering

## 这一篇解决什么问题

让每个 AI 会话总是拿到正确的最小上下文：项目规则、当前任务、对话三者分层，避免上下文污染，也避免规则只存在于某个人的脑子里或某次对话里。

## 为什么这样设计

长对话后期，Agent 依赖对话记忆而不是项目事实，容易编造约束或违反已批准的决策。上下文分三层，让新会话从文档重建事实：

- **Project Context**：仓库级事实——架构、安全、运维、测试规则；
- **Feature Context**：当前功能的事实——Spec、Ticket、相关 ADR 与 API；
- **Conversation Context**：仅当前 Ticket 的对话。

对应原则：

> One Ticket = One Conversation（一个 Ticket 一个会话）

## 怎么执行

1. **AGENTS.md 是索引，不是百科全书**：控制在几百行内，列出项目定位、技术栈、必读文档与铁律。示例骨架：

   ```text
   # Project
   这是一个 Kubernetes 操作平台。

   # Stack
   Frontend: React  /  Backend: FastAPI  /  DB: PostgreSQL

   # Required Reading
   Architecture: docs/ARCHITECTURE.md
   Security: docs/SECURITY.md
   Ops rules: docs/OPS.md
   Testing: docs/TESTING.md

   # Development
   Each ticket uses a fresh session.
   Use TDD.
   Do not weaken authentication.
   Do not bypass audit logging.

   # Verification
   Run: ...
   ```

2. **详细规范放 docs**：`ARCHITECTURE.md`、`SECURITY.md`、`OPS.md`、`TESTING.md`，Agent 按需读取，不常驻对话。
3. **每个 Ticket 开新会话**：读 AGENTS.md → 读 Ticket → 读相关 Spec/ADR → 开始 TDD。
4. **禁止**：把多个 Ticket 塞进同一会话；把整个代码库粘贴进对话。

## 常见错误

- 3000 行 AGENTS.md：每次会话烧掉大量 token，规则反而没人读；
- 规则只写在对话里：新会话一无所知，会话一丢规则就丢；
- 一个会话做多个 Ticket：上下文污染，验收证据串台；
- AGENTS.md 只有项目简介，没有必读文档索引与验证命令。

## 门禁与下一步

- **门禁**：新会话凭文档可以独立开工；停止条件已写在 AGENTS.md；
- **下一步**：[TDD and Feedback Loop](05-tdd-and-feedback-loop.md)，用反馈循环实现 Ticket。

## 相关文档

- [Full Track §10](../workflows/full.md)：TDD 阶段如何消费上下文
- [Artifact Map](../references/artifacts.md)：Tickets 是会话的事实来源
