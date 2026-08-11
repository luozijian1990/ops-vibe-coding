# E2E and Human Acceptance

## 这一篇解决什么问题

在真实浏览器验证"用户能否完成整条工作流"，并让人做最终验收。AI 的测试结论不能替代人的业务与风险判断，这是整条 Golden Path 的收口。

## 为什么这样设计

E2E 的核心对象是 **User Workflow（用户工作流）**，不是 UI Element。测"按钮存在、页面能打开、文本出现"发现不了流程断裂。

同时保持 HITL（Human In The Loop）：测试计划、破坏性操作权限、最终验收都由人批准。防止出现"AI 自己设计测试 → 自己跑 → 自己宣布 PASS"的自证循环。

## 怎么执行

1. **生成测试任务**：由 Spec / Tasks 生成 `test-task.md`，等待人工批准后再运行。
2. **优先测试真实工作流**。以 Kubernetes 平台为例，测试这条链路而不是散落的按钮：

   ```text
   登录 → Cluster → Deployment → Restart
   → Confirm → Progress → Success → 审计记录可见
   ```

3. **失败先分类**：分环境、测试、产品、数据或权限问题，修复需要额外授权，然后重跑。
4. **输出报告**：Test Plan、证据（截图 / DOM）、`PASS / FAIL / SKIPPED`、环境限制。
5. **人工验收**：由人回答——

   > 这个软件是否解决了最初的问题？失败行为、风险和运行成本是否可以接受？

## 常见错误

- 只测"页面能打开、文本出现"：发现不了流程断裂；
- AI 自己设计测试、自己跑、自己宣布 PASS，且没有留下证据；
- 破坏性 E2E（真实删除资源）没有单独批准；
- FAIL 不分类就乱修：改错方向浪费一整轮；
- 把"测试通过了"等同于"产品对了"：验收是人的决定。

## 门禁与下一步

- **门禁**：Test Report 满足验收条件，且人明确接受（接受、补充或退回）；
- **回退**：原型流程不合理回 Prototype；E2E 失败先分环境再行动（完整回退规则见 [Full Track](../workflows/full.md)）；
- 这是闭环终点，无下一步。

## 相关文档

- [Full Track §13~14](../workflows/full.md)：E2E 与 Human Acceptance、回退规则
- [Artifact Map](../references/artifacts.md)：E2E 报告与验收证据
- [Skill Map](../references/skills.md)：`playwright-e2e-debug-report`
