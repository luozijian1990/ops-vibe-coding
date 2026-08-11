# FAQ

## 一定要从 HTML 开始吗？

不强制。已经会写 PRD 或正在改造现有系统的，可以直接从后续阶段进入。但从未做过产品的人，HTML 是最低成本、最直接的意图验证方式。见 [Prototype First](02-prototype-first.md)。

## 不会 React 可以吗？

可以。你只负责确认原型和最终验收，转换由 `html-to-mui-react` 的 fidelity 模式完成。见 [Prototype First](02-prototype-first.md)。

## 一定要用 MUI 吗？

不是。组件库服从已批准的原型；只有明确说视觉无需保留时，才统一为 MUI 风格。见 [Prototype First](02-prototype-first.md)。

## 一定要用 Python / Go 吗？

不是。技术栈由 UI 暴露的能力加上 Grill 确认的约束共同决定。见 [Architecture and SDD](03-architecture-and-sdd.md)。

## 小项目需要完整 SDD 吗？

不需要。Lightweight Track 会缩短文档与审查深度，但不能跳过产品意图和架构约束的人工确认。见 [Lightweight Track](../workflows/lightweight.md)。

## 什么时候可以跳过 TDD？

工具纯一次性、用完即弃、无生产价值时，可以只保留最轻验证。一旦涉及生产资源、敏感操作或长期维护，回到 TDD。见 [TDD and Feedback Loop](05-tdd-and-feedback-loop.md)。

## 什么时候需要 Worker？

存在长时间运行、需要重启恢复、独立于请求的异步任务，且 Grill 确认了重试、并发、恢复约束时才需要。UI 有进度条不等于需要 Worker。见 [Architecture and SDD](03-architecture-and-sdd.md)。

## 什么时候需要 Redis？

有明确证据需要缓存、队列或分布式锁时才需要。默认能用数据库解决的问题，不要加组件。见 [Architecture and SDD](03-architecture-and-sdd.md)。

## 什么时候需要 Agent？

需要远程在目标主机执行任务且不能内嵌在服务里，同时确认了凭证隔离、命令白名单与审计时才需要。见 [Ops Readiness Review](06-ops-readiness.md)。

## React Mock 可以直接决定后端吗？

不能。UI 只暴露能力候选，部署、规模、安全、恢复等约束必须经 Grill 确认。见 [Architecture and SDD](03-architecture-and-sdd.md)。

## 为什么每个 Ticket 开新会话？

减少上下文污染，让 Agent 从文档重建事实，而不是从旧对话继承噪音。见 [Context Engineering](04-context-engineering.md)。

## 为什么 AI 测试完还要人工验收？

AI 能证明"按规格工作"，但"是否解决最初的问题、风险是否可接受"是人的业务判断。见 [E2E and Human Acceptance](07-e2e-and-human-acceptance.md)。

## Ops Readiness 和 Code Review 有什么区别？

Code Review 查代码正确性与规范符合度；Ops Readiness 查生产运行与操作安全，两者互补不替代。见 [Ops Readiness Review](06-ops-readiness.md)。

## Prototype 和 PRD 有什么区别？

Prototype 是可点、可看、可体验的契约；PRD 是文字描述。对非产品背景的运维工程师，前者表达意图更直接。见 [Prototype First](02-prototype-first.md)。
