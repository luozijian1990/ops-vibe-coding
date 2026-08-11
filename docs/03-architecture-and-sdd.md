# Architecture and SDD

## 这一篇解决什么问题

React Mock 完成之后，如何不靠脑补得到架构与规格：UI 暴露能力，Grill 暴露约束，两者共同决定架构；Spec 再把架构变成可测试的精确行为。

## 为什么这样设计

前端只能说明系统"需要什么能力"，不能单独决定 Redis、MQ、Worker、WebSocket、微服务。三份契约分工明确：

| 契约 | 回答的问题 | 产生方式 |
|---|---|---|
| Prototype | 用户怎么使用 | 人工体验后批准 |
| Architecture | 系统由什么组成 | Draft → Grill → Final |
| Spec | 系统精确怎么工作、怎么验证 | 消费 Prototype 与 Architecture |

对应方法论：

> UI 揭示能力，Grill 揭示约束，架构来自两者。

## 怎么执行

1. **Draft**：第一次调用 `stack-selector` 生成 `stack-selection.draft.md`，只记录 React Mock 暴露的能力证据、基于已知约束的最小暂定架构、以及会改变架构的开放问题。
2. **Grill**：确认部署位置、用户与数据规模、外部系统、认证来源、权限模型、审批审计、凭证存放、任务时长与重试恢复、数据留存、网络边界、可复用基础设施。以 Kubernetes 操作平台为例：集群凭证存在哪？删除要审批吗？操作审计保留多久？目标集群不可达怎么办？
3. **Final**：第二次调用 `stack-selector` 消费同一 Draft 与 Grill 决策，生成 `stack-selection.md`，每条组件决策都要能映射到能力或约束。
4. **Spec**：用 `to-spec` 生成可执行契约，覆盖功能、API、数据模型、状态机、错误处理、授权、审计、可观测性、失败行为、测试 seam、验收条件。Prototype 与 Architecture 都不替代 Spec。
5. **Tickets**：用 `to-tickets` 按用户价值纵向切分。Kubernetes 平台例子：
   - 用户查看 Cluster List；
   - 用户查看 Cluster Detail；
   - 用户执行 Deployment Restart 并看到进度；
   - 用户查看 Restart 审计记录。

   不要切成"写数据库、写 API、写页面"。

## 常见错误

- 看到实时刷新就上 WebSocket，看到进度条就上 Worker：先问约束，再选组件；
- Grill 只问页面按钮，不问部署、规模、失败恢复：架构建立在无人确认的假设上；
- Spec 写成页面描述或 PRD 复述：不能指导测试的 Spec 是废纸；
- 关键约束未知仍声称 Final：未知项保持 Draft，项目只能算 Prototype。

## 门禁与下一步

- **门禁**：会改变架构的关键约束已由人确认；Spec 范围与验收条件获得批准；
- **下一步**：[Context Engineering](04-context-engineering.md) 设计项目上下文，进入一个 Ticket 一个会话的开发。

## 相关文档

- [Full Track §5~9](../workflows/full.md)：Draft、Grill、Final、Spec、Tickets
- [Artifact Map](../references/artifacts.md)：`stack-selection.draft.md`、`stack-selection.md`、Spec、Tickets
- [Skill Map](../references/skills.md)：`stack-selector`、`grill-me`、`to-spec`、`to-tickets`
