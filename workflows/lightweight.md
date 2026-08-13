# Lightweight Track

适合第一次体验、Demo 和低风险小工具。它减少文档深度和审查层级，但保留会改变架构与风险的人工决定。

```text
HTML Prototype
→ Human Review
→ React + Mock
→ Prototype Parity
→ Architecture Draft
→ Lightweight Grill
→ Architecture Final
→ Minimal Spec
→ Vertical Tickets
→ Implement + Test
→ Browser Workflow Test
→ Human Acceptance
```

## 最小执行方式

1. 用一个 HTML 文件表达核心用户流程，让至少一名真实使用者确认。
2. 转成 React + Mock，执行 build，并在真实浏览器完成该流程。
3. 执行 Prototype Parity 检查。至少确认核心布局没有明显偏离、关键交互保持一致、主要 Workflow 可以执行，且已批准的产品意图没有在工程化转换中丢失。
4. 从 Mock 生成 Architecture Draft；只记录最小建议，不从 UI 直接引入 Redis、MQ、Worker 或微服务。
5. 用一次 Lightweight Grill 确认部署位置、用户与数据规模、外部系统、认证授权、失败处理和可复用基础设施。
6. 消费同一 Draft 生成 Architecture Final。关键未知项未解决时继续保持 Draft，项目只能视为 Prototype。
7. 编写 Minimal Spec，至少覆盖 Core behavior、API / Data contract、Authorization、Error behavior 和 Acceptance criteria。
8. 用纵向 Ticket 实现；每个 Ticket 至少有一个可失败的验收测试或可复现验证步骤。
9. 在浏览器运行完整 Workflow，由人决定是否接受。

## 什么时候升级到 Full Track

出现任一条件就使用 [Full Track](full.md)：

- 操作生产基础设施、执行远程命令或修改 Kubernetes 资源；
- 涉及 Secret、客户数据、多租户、审批或细粒度权限；
- 存在长任务、自动重试、重启恢复、高并发或多实例；
- 工具由团队长期维护，或者失败会造成明显业务影响；
- 需要正式发布、生产就绪声明或审计证据。

## 不代表什么

Lightweight Track 减少的是文档深度和审查深度，不是关键产品契约。它不是免测试、免架构确认或生产就绪捷径，不能跳过 Parity、Draft → Grill → Final、Minimal Spec，也不能让 Agent 代替人接受风险。
