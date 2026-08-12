# Feedback and Self-Refinement

> 让团队的每一次纠错，都变成未来会话的上下文。

## 这一篇解决什么问题

AI 模型没有跨会话记忆。上次犯的错，下次新会话可能再犯；上次和某个新人讲清楚的约束，下次换个人又得重新讲。纠错如果只在对话里发生一次，就永远只是一次。

## 闭环

```text
Agent makes mistake
        ↓
Human corrects it
        ↓
Is this reusable knowledge?
        ↓
      Yes
        ↓
Agent proposes where to record it
        ↓
Human approves
        ↓
Update project knowledge
        ↓
Future sessions inherit it
```

## 核心原则

AI 不应该自动修改长期规则。沉淀必须经过：

```text
Propose
→ Human Approve
→ Persist
```

沉淀到哪里，根据内容决定：

```text
AGENTS.md         全局硬规则
docs/             架构 / 安全 / 运维 / 测试规范
experience/       踩坑经验
sop/              标准操作规程
Skill             可复用工作流
```

采用作用域化 Context 时，可以进一步定位：

```text
context/team/                                   跨项目团队规范
context/harness-framework/                      研发治理入口或版本引用
context/project/{project}/{module}/{service}/   服务长期知识
.../experience/                                 有来源、有范围的历史经验
IDL                                             服务接口契约
.service-matrix/dependencies.yaml               跨服务关系
```

规则进入哪个位置取决于其适用范围，而不是发现错误时碰巧在哪个目录工作。修改 IDL 或服务矩阵也必须经过 `Propose → Human Approve → Persist`，并执行相应兼容性验证。

## 怎么判断是否值得沉淀

值得沉淀：

```text
重复出现的问题
项目级固定约束
架构决策
安全边界
运维规范
测试规范
容易再次踩坑的经验
```

不值得沉淀：

```text
一次性 Bug
临时调试信息
当前 Ticket 特有状态
偶然实现细节
```

## Ops 风格的例子

用户发现 AI 创建的生产变更 API 没有审计。

第一次：

```text
Human: 生产变更必须记录 operator、target、action、request_id 和 result。
```

Agent 判断这是长期 Ops Rule，于是提出：

```text
是否将此规则写入 docs/operations.md？
```

Human 批准后持久化。以后新 Ticket、新会话、新 Agent Runtime 都读取该规则。

> Harness 的目标不是让 AI 自动学习，而是让团队经验能够被版本化、审查和重复使用。

## 常见错误（Anti-patterns）

- **AI 自己改 AGENTS.md 铁律**：没有人工批准，规则被静默削弱；
- **什么错误都沉淀**：一次性 Bug 也写进规则，垃圾堆积，规则失焦；
- **纠错只留在对话里**：会话结束，教训消失，下次重蹈覆辙；
- **沉淀但没人 review**：经验没被验证就入库，误导后续会话。
- **把经验当现行契约**：历史背景可以放 `experience/`，当前强约束必须提升到规则、ADR、IDL 或 Service Matrix；

## 门禁与下一步

- **门禁**：每条沉淀都有"Propose → Human Approve → Persist"的完整记录；
- **下一步**：[06-tool-and-permission-boundaries.md](06-tool-and-permission-boundaries.md)，定义 AI 能做什么、有权做什么。
