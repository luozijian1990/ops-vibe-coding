# Runtime Portability

> Tools are replaceable. Engineering context is durable.

## 这一篇解决什么问题

今天用 Claude Code，明天可能换 Codex、OpenCode、Gemini CLI 或 Cursor。如果流程、规则、知识和证据都存在某个 AI 产品的聊天历史里，换工具就等于从零开始。

## 核心资产应该驻留在 Repository

```text
Specs
Tickets
AGENTS.md
Docs
Gate Reports
Test Reports
Architecture
Ops Rules
Scoped Context
IDL
Service Matrix
```

这些全部是仓库里的版本化文件，而不是某个 AI 产品的会话记录。仓库在哪里，工程资产就在哪里。

## 换工具时什么不变

```text
Workflow 不变          流程定义在 workflows/
Artifacts 不变          产物定义在 references/artifacts.md
Rules 不变              规则在 AGENTS.md + docs/
Acceptance Criteria 不变 验收条件在 Spec 和 Ticket 里
Service Contracts 不变   接口在 IDL，关系在 Service Matrix
```

新的运行时只需要做一件事：**读同一套仓库资产**。

## 运行时差异的处理

不同工具读 AGENTS.md、Skills 的方式略有差异，第一版不做复杂 Runtime Adapter。建议：

1. 把规则和知识放在仓库（工具无关）；
2. 按当前工具的要求，在工具配置目录只放入口、引用或指向仓库资产的链接；
3. 如果运行时只能消费生成文件，将其标记为不可手工编辑，并从仓库权威来源重复生成。

示例：

```text
.claude/    ← 入口，指向仓库内的 AGENTS.md 与 docs/
.codex/     ← 入口，指向同一份
.opencode/  ← 入口，指向同一份
```

正文只有一份，入口可以有多个。

## 怎么执行

1. 所有规范、知识、门禁产物写进仓库，接受 code review 与版本控制；
2. 每个会话从仓库读取，不从记忆恢复；
3. 换工具时验证"新工具凭仓库资产能独立开工"，把验证记录写进仓库。

如果团队规范来自独立仓库或制品，项目必须记录其来源和版本。不同 Runtime 解析的是同一固定版本，不能各自读取不受控的“最新版”。

## 常见错误（Anti-patterns）

- **规则只存在 Cursor 的自定义指令里**：换工具，规则清零；
- **Spec 只写在聊天记录里**：会话一关，验收标准消失；
- **为某个工具写专属工作流**：绑定到特定产品，团队迁移成本高；
- **过早实现复杂 Runtime Adapter**：第一版用 Markdown + 仓库就够，工具适配按需再做。

## 门禁与下一步

- **门禁**：从仓库根目录读 AGENTS.md 的任何一个运行时，都能理解项目并开工；
- **下一步**：回到 [01-context-engineering.md](01-context-engineering.md) 检查上下文分层是否完整，或在 [examples/](examples/) 里找到可直接改写的模板。
