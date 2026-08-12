# Agents and Instructions

> AGENTS.md 是项目上下文索引和关键硬规则入口，不是整个项目知识库。

## 这一篇解决什么问题

让每个 Agent（Claude Code、Codex、OpenCode 等）在启动时就获得一致的行为基线：项目是什么、规矩是什么、去哪里找细节、怎么验证。同时避免"规则散落在 README、AGENTS、Workflow、Skill 各维护一份，逐渐漂移"。

## AGENTS.md 的职责

AGENTS.md 只做两件事：

```text
1. 索引：告诉 AI 去哪里读什么
2. 硬规则：少量不可协商的规则直接写在这里
```

详细规范放在 `docs/`，AGENTS.md 引用它们，而不是复制它们。

```text
Rules should be referenced, not duplicated.
```

## 建议结构

```text
Project Overview    项目是什么、为谁服务
Stack               技术栈
Required Reading    必读文档索引（架构/安全/运维/测试）
Development Workflow 开发流程要点
Hard Rules          铁律（不可违反）
Testing             测试命令与要求
Security / Ops      安全与运维约束
Verification Commands 验证命令
```

示例见 [examples/AGENTS.example.md](examples/AGENTS.example.md)。

## 怎么执行

1. 项目启动时写第一版 AGENTS.md，只放索引和铁律；
2. 详细规则进 `docs/`，AGENTS.md 引用路径；
3. 每次会话开始，Agent 读 AGENTS.md，按 Required Reading 按需加载；
4. 规则变更走 code review，像代码一样可 diff、可回滚。

如果采用 `context/` 作用域布局，AGENTS.md 应明确启动顺序，但不要复制规则正文：

```text
Always Read
→ context/team/
→ context/harness-framework/

Then Read by Ticket Scope
→ context/project/{project}/{module}/{service}/
→ Spec / Ticket / relevant IDL
→ .service-matrix/dependencies.yaml（涉及跨服务变更时）
```

“自动继承”是启动协议的结果，不是目录名称的能力。任何受支持的 Agent Runtime 都必须能从 AGENTS.md 找到同一套入口。

## 单一真相源（Single Source of Truth）

每类规则尽量只有一个权威来源：

```text
Full Workflow       → workflows/full.md
Skill Mapping       → references/skills.md
Artifact Definition → references/artifacts.md
Ops Readiness       → docs/06-ops-readiness.md
Interface Shape      → IDL（OpenAPI / Protobuf 等）
Service Relationship → .service-matrix/dependencies.yaml
```

其他文档引用即可，不要重复维护整套定义。一次修改，全项目保持相同语义。

## 常见错误（Anti-patterns）

- **3000 行 AGENTS.md**：每轮会话烧掉大量 token，规则反而没人读；
- **README、AGENTS、Skill 各写一份规则**：改一处漏两处，AI 面对互相矛盾的规则只能猜；
- **AGENTS.md 只有项目简介**：没有必读文档索引，也没有验证命令，AI 不知道去哪里找细节、怎样算完成；
- **把一次性决定写进铁律**：只有频繁出现、代价高、不可协商的才值得进入硬规则。

## 门禁与下一步

- **门禁**：新会话读完 AGENTS.md 能回答"这个项目怎么验证、哪里不能碰"；
- **下一步**：[03-task-and-session-lifecycle.md](03-task-and-session-lifecycle.md)，定义任务如何在一个会话内执行并留下证据。
