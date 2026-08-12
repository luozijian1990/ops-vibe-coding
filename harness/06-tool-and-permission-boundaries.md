# Tool and Permission Boundaries

> Capability does not imply permission. AI 有能力执行，不代表自动获得执行权限。

## 这一篇解决什么问题

Ops 场景里 AI 能触达的东西往往是有真实后果的：生产部署、数据库变更、远程 Shell、Kubernetes 删除、告警操作。Agent 一旦拥有执行能力就自动获得全部权限，等于把生产环境的钥匙直接交给一个概率模型。

## 操作类型

| 类型 | 例子 | 基本要求 |
|---|---|---|
| **Read** | 读取代码、日志、Manifest、测试结果 | 确认数据敏感度和目标环境 |
| **Write** | 修改当前仓库、生成测试文件、修改配置 | 明确可写范围，保留现有用户修改 |
| **Execute** | `npm test`、`pytest`、`go test`、lint | 明确命令副作用，记录可复现结果 |
| **External Access** | 调用远程 API、下载依赖、访问集群 | 确认目标、凭证用途和数据边界 |
| **Production / Destructive** | 生产部署、数据库变更、远程 Shell、批量执行、Secret 修改、真实告警/Incident 操作、`kubectl delete` | **禁止自动执行，必须 HITL** |

操作类型本身不等于风险等级。读取生产 Secret 仍是高风险；名为测试的 Playwright 流程也可能触发真实变更。执行前同时判断：

```text
Action          要做什么
Target          本地、测试还是生产
Scope           影响一个文件、一个资源还是一批资源
Reversibility   能否可靠撤销
Data            是否涉及 Secret、客户数据或敏感日志
Approval        当前授权是否明确覆盖这次操作
```

## 怎么执行

1. **AGENTS.md 声明边界**：写清允许的目标、作用范围和必须人工批准的操作（见 [02-agents-and-instructions.md](02-agents-and-instructions.md)）；
2. **执行前检查实际副作用**：不能只按工具名或命令名授权；
3. **Agent 遇到越界操作时停下来提问**，而不是尝试绕过；
4. **执行类命令留下证据**：命令、环境、输出和结果写进验证记录；
5. **高危操作默认拒绝**，只有人的批准明确覆盖目标和范围后才执行。

## 与 Ops Readiness 的分工

```text
Harness
→ 控制 Agent 开发过程中能做什么（权限边界）

Ops Readiness Review
→ 检查开发出来的软件上线后是否安全可靠（产品风险）
```

不要混在一起：权限边界管"AI 现在能不能执行这个命令"，Ops Readiness 管"这个软件上线后会不会出事"。

## 常见错误（Anti-patterns）

- **能执行 = 可以执行**：Agent 有 `kubectl` 权限就随便跑 `kubectl delete`；
- **按命令名授权**：看到 `playwright` 就默认安全，却没有检查测试环境和实际副作用；
- **权限写在对话里**：新会话不知道边界，规则随会话消失；
- **高危操作做成"先执行再汇报"**：数据库变更出了事，汇报已经来不及了；
- **把权限边界当成 Ops Readiness**：AI 没删生产资源，不代表生产软件没问题。

## 门禁与下一步

- **门禁**：AGENTS.md 明确目标、范围和审批边界；Production / Destructive 操作必须人工批准，且 Agent 不会静默越界；
- **下一步**：[07-runtime-portability.md](07-runtime-portability.md)，确保换工具后工程资产不丢失。
