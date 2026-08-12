# Context Engineering

> 不要依赖用户每次重新 Prompt AI。长期有效的知识应该沉淀为版本化文件。

## 这一篇解决什么问题

让每个 AI 会话总是拿到正确的最小上下文，同时让团队知识可以保存、版本化、检查、复用。

如果知识只存在于聊天记录或某个人脑子里，新会话、新成员、新模型每次都要从零开始，且每次得到的答案都可能不同。

## 三层上下文模型

| 层 | 生命周期 | 存放位置 | 内容 |
|---|---|---|---|
| Project Context | 长期有效 | `AGENTS.md` + `docs/` | 架构、安全边界、代码规范、部署方式、运维规则、测试规则 |
| Feature Context | 当前功能周期 | `spec/`、`ticket/` | Spec、Ticket、架构决策、API 契约 |
| Session Context | 仅本次会话 | 会话内 | 当前 Ticket 需要的具体文件与对话 |

## 核心原则

```text
Load only what the current task needs.
```

不要把所有文档一次性塞进 Context Window。模型能力再强，上下文一团糟也只会产出与上下文一样糟的结果。

```text
One Ticket = One Context = Preferably One Fresh Session
```

## 两个正交维度

三层上下文描述知识的**生命周期**。团队规模扩大或仓库包含多个服务时，还需要描述知识的**作用范围**：

| 作用范围 | 推荐位置 | 谁需要遵守 | 典型内容 |
|---|---|---|---|
| Team | `context/team/` | 团队维护的所有项目 | Git 规范、错误码空间、日志规范 |
| Harness Governance | `context/harness-framework/` | 使用同一研发治理协议的项目 | Workflow 入口、Gate 规则、文档模板版本 |
| Project / Module / Service | `context/project/{project}/{module}/{service}/` | 对应范围内的任务 | 架构、接口、Runbook、历史决策、踩坑经验 |

这套目录是大型、多模块或多服务项目的**推荐布局**，不是所有项目的强制结构。小项目继续使用 `AGENTS.md + docs/` 即可；没有 Module 或 Service 时应省略对应层级，不要制造空目录。

```text
Lifecycle: Project / Feature / Session  → 这份知识能活多久
Scope:     Team / Governance / Service → 这份知识约束谁
```

文件存在不等于 Agent 会自动继承。根 `AGENTS.md` 或运行时启动入口必须索引 Always Read 内容，再按当前 Ticket 加载相关 Project / Module / Service Context。跨仓库共享团队规范时，应引用一个经过评审并固定版本的来源；不要在每个仓库手工复制规则正文。

## Knowledge as Code

不同事实使用不同表达：

```text
团队规则、历史决策、Runbook  → Markdown，便于阅读和评审
接口形状                     → OpenAPI / Protobuf / 其他 IDL
跨服务关系                   → .service-matrix/dependencies.yaml
验证结果                     → Test Report / Gate Record
```

IDL 是服务接口的权威来源；Service Matrix 只记录 IDL 无法完整表达的关系，例如 owner、consumer、调用方向、关键性和契约位置。不要在两处重复维护字段定义。

当 Ticket 改变接口或依赖时，Session Context 应加载相关 IDL 和矩阵条目。影响面分析先查结构化关系，再用代码搜索验证动态调用、配置引用等未被契约覆盖的情况。矩阵提高确定性，但不能把搜索和运行证据完全删除。

## 怎么执行

### 1. Project Context（长期，稳定）

```text
AGENTS.md

docs/
├── architecture.md
├── security.md
├── operations.md
└── testing.md
```

Project Context 描述"这个项目永远为真的东西"。它是所有会话共享的基线。

大型项目也可以把长期知识按作用域放进 `context/`。其中历史决策和踩坑经验可落在：

```text
context/project/{project}/{module}/{service}/experience/
```

每条经验应写清适用范围、事实来源和日期。仍然有效的架构约束应进入 ADR 或架构文档，不能只藏在经验目录。

### 2. Feature Context（当前功能）

```text
spec/
└── {ticket-id}.spec.md

ticket/
└── {ticket-id}.md

docs/adr/
└── {adr-id}-*.md
```

Feature Context 描述"当前这个功能相关的决策与契约"。一个功能完成并验证后，有价值的部分要么合并进 Project Context，要么保留为历史记录。

### 3. Session Context（当前会话）

只有当前 Ticket 需要的：

```text
AGENTS.md
→ 当前 Ticket
→ 相关 Spec / ADR
→ 相关代码片段
```

## 示例

一个 Kubernetes 变更 Ticket 的上下文加载顺序：

```text
1. 读 AGENTS.md（索引）
2. 读 ticket/k8s-restart.md（任务）
3. 读 spec/k8s-restart.spec.md（验收条件）
4. 读 docs/security.md + docs/operations.md（相关规则）
5. 开始实现
```

不是所有任务都读取所有文档。一个纯前端 Ticket 不需要读 `docs/operations.md`，除非它涉及权限或部署。

## 常见错误（Anti-patterns）

- **把所有文档贴进对话**：Token 烧光，模型反而分不清哪些是当前任务的；
- **规则只写在对话里**：会话一丢，规则就丢，下一个会话一无所知；
- **AGENTS.md 堆成百科全书**：索引退化，每轮会话都读几万字，没人遵守；
- **一个会话做多个 Ticket**：上下文污染，验收证据串台；
- **目录存在就等于自动继承**：没有 AGENTS.md 或启动入口索引，Agent 根本不知道要读；
- **把 Service Matrix 当完整事实**：动态调用和运行时配置可能不在表内，仍需搜索和验证；
- **在每个仓库复制团队规则**：副本逐渐漂移，无法确定哪一份权威。

## 门禁与下一步

- **门禁**：新会话凭 AGENTS.md + Ticket + 相关 Spec 可以独立开工；
- **下一步**：[02-agents-and-instructions.md](02-agents-and-instructions.md)，把长期规则放进 AGENTS.md 并保持它是索引而非垃圾场。
