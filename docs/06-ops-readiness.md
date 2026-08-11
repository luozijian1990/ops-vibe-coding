# Ops Readiness Review

## 这一篇解决什么问题

运维软件在"能跑"之外，是否具备生产运行与操作安全基础：危险操作、权限、审计、凭证、远程执行、失败恢复。这是 Ops Vibe Coding 区别于通用开发工作流的核心阶段。

## 为什么这样设计

运维工具能删资源、重启服务、执行命令、触碰生产数据。权限、审计、Secret、恢复不是上线前临时补的补丁，而是设计的一部分。

`ops-readiness-review` 与 `code-review` 分工明确：

```text
code-review        → code correctness（代码正确性）
ops-readiness-review → operational correctness（运行与操作安全）
```

两者互补，不要互相塞内容。

## 怎么执行

1. **输入**：AGENTS.md、Spec、Architecture、stack-selection、相关 Tickets、关键代码、部署清单、测试结果。
2. **按风险检查**（允许 `NOT_APPLICABLE`，不要求所有项目具备全部能力）：
   - **A. 危险操作**（Delete / Restart / Scale / Drain / Evict / Execute）：确认、授权、审计、幂等、爆炸半径、回滚；
   - **B. 认证授权**：后端强制校验，禁止只靠隐藏按钮实现权限；
   - **C. 审计**：Who / When / What / Target / Result / Request ID，不记录敏感凭证；
   - **D. Secret**：存储、加密、日志泄漏、前端泄漏、轮换、范围；
   - **E. 远程执行**：命令白名单、超时、并发、凭证隔离、输出限制、取消、审批、目标校验；
   - **F. Kubernetes**：最小 RBAC、集群凭证隔离、API 超时、分页、限流、重试、resourceVersion 处理；
   - **G. 异步任务**：状态持久化、幂等、重试、超时、取消、重启恢复、防重复执行、并发上限；
   - **H. API 可靠性**：超时、分页、输入校验、错误响应、限流、Request ID；
   - **I. 可观测性**：结构化日志、health / readiness、指标、错误可见、请求关联；
   - **J. 数据生命周期**：保留、清理、备份、恢复、迁移、删除、日志增长；
   - **K. 部署**：配置与 Secret 分离、健康检查、优雅退出、资源限制、迁移、回滚；
   - **L. 失败/恢复**：DB 挂了？目标集群不通？Worker 重启？任务重复执行？

   以 Kubernetes 平台为例，Restart 操作必须回答：谁能执行、是否审计、集群凭证存在哪、目标不可达怎么办、并发上限多少。
3. **输出** `docs/reviews/YYYY-MM-DD-ops-readiness.md`，格式为 `PASS` / `PASS_WITH_RISKS` / `FAIL`，分 Blockers、Major、Minor、Accepted Risks、Not Applicable、Recommended Verification、Evidence。
4. **严重级别**：`BLOCKER` / `MAJOR` / `MINOR` / `INFO`。BLOCKER 示例：远程命令无权限控制、删除资源无审计、Secret 进前端、跨租户数据可访问、任务无限并发。

## 常见错误

- 对只读 Dashboard 强制要求审批，对普通 CRUD 强制 MQ/Redis，对所有项目强制 OTel/HA：复杂度必须由证据驱动；
- 把 Ops 检查塞进 code-review：两者都会变浅；
- 只查代码，不查部署清单与测试证据；
- BLOCKER 没修就让 Agent 宣布"风险已接受"：接受风险是人的决定，不是 AI 的。

## 门禁与下一步

- **门禁**：无未修复且未获授权接受的主要风险，关键控制有足够证据；
- **下一步**：[E2E and Human Acceptance](07-e2e-and-human-acceptance.md)，用真实浏览器验证完整用户流程。

## 相关文档

- [Full Track §12](../workflows/full.md)：Ops Readiness 阶段
- [Artifact Map](../references/artifacts.md)：`docs/reviews/*-ops-readiness.md`
- [Skill Map](../references/skills.md)：`ops-readiness-review`
