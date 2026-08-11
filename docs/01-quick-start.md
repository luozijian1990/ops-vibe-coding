# Quick Start

用一个小型内部运维工具跑通第一条链路。预计重点是理解产物与门禁，不是第一轮就达到生产就绪。

## 1. 写下问题

用三句话描述：谁遇到什么问题、现在如何处理、完成后怎样算有价值。暂时不要决定微服务、Redis、MQ 或具体框架。

## 2. 选择或制作 HTML Prototype

至少包含一个完整用户流程，以及 Loading、Error、Empty、Success 中与该流程有关的状态。让真实使用者点击体验，记录批准的页面、交互和目标 viewport。

输出：

```text
prototype/*.html
prototype/approval.md
```

门禁：人明确批准产品意图。

## 3. 转成 React + Mock

默认用 `html-to-mui-react` 的 `fidelity` 模式。只有用户明确说原型视觉无需保留时，才选择 `mui-normalize`。

转换前确认页面、路由、公共布局、关键流程、非默认状态和目标 viewport。转换后运行 build，并用真实浏览器走关键流程。

门禁：`prototype-parity-review` 为 `PASS`，或由人接受 `PASS_WITH_NOTES`。

## 4. 先 Draft，再确认架构

第一次调用 `stack-selector` 只生成 Architecture Draft。它记录：

- React Mock 暴露的能力证据；
- 基于已知约束的最小架构暂定建议；
- 会改变语言、部署单元、安全或可靠性的 Grill 问题。

用 Grill 确认部署位置、规模、外部系统、认证与权限、任务可靠性、失败恢复和已有基础设施。第二次调用 `stack-selector` 消费同一 Draft 和确认结果，才可生成 Final。

门禁：关键约束仍未知时保持 Draft，不能进入 SDD。

## 5. 写最小 Spec 和纵向 Tickets

Spec 至少覆盖功能、API、数据、状态、错误、授权、审计、失败行为、测试 seam 和验收条件。Ticket 按用户价值纵向切分，例如“用户执行一次重启并看到审计记录”，不要拆成“写数据库、写 API、写页面”。

## 6. 一个 Ticket 一个会话

每个 Ticket 使用新会话，读取项目索引、Spec、Ticket 和必要 ADR，按 RED → GREEN → REFACTOR → VERIFY 工作。停止条件必须是测试、lint/build 和验收条件，而不是 Agent 自我判断。

## 7. 浏览器验证并由人验收

Lightweight Track 至少真实运行一次完整用户流程并保存结果。它不等于生产就绪；如果工具涉及敏感操作、生产资源或长期团队使用，继续执行 Full Track 的代码审查、Ops Readiness 和正式 E2E 门禁。

最后由人回答：

> 这个软件是否解决了最初的问题，失败行为、风险和运行成本是否可以接受？
