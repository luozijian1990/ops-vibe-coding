# 项目

内部 Kubernetes 运维平台。

# 技术栈

前端：React
后端：FastAPI
数据库：PostgreSQL

# 必读文档

架构：
docs/architecture.md

安全：
docs/security.md

运维：
docs/operations.md

测试：
docs/testing.md

团队规范：
context/team/

Harness 治理规范：
context/harness-framework/

服务上下文：
只加载 `context/project/` 下与当前 Ticket 匹配的路径。

服务契约：
多服务项目以 IDL 作为接口的单一真相源。涉及跨服务变更且项目维护了
`.service-matrix/dependencies.yaml` 时，读取对应关系并验证受影响的消费者。
小型或单服务项目无需为使用 Harness 而引入 Service Matrix。

# 开发规则

- 一个 Session 只处理一个 Ticket。
- 实现过程使用 TDD。
- 不得绕过后端授权。
- 生产变更必须记录审计日志。
- 没有证据，不增加基础设施。

# 权限边界

读取：
仓库内非敏感文件可直接读取；敏感数据和生产信息需要确认授权。

写入：
允许修改当前仓库，但必须保留无关的现有修改。

执行：
允许在本地执行 `npm test` 和 `pytest`，必须记录结果。

外部访问：
访问前确认目标、凭证用途和数据边界。

E2E：
执行 `npx playwright test` 前批准测试范围和环境。生产或破坏性流程需要
单独的人工批准。

生产 / 破坏性操作：
生产部署、数据库迁移、远程 Shell、`kubectl delete` 和 Secret 修改必须获得
人工批准。未获批准时停止并询问。

# 验证命令

前端：
npm test

后端：
pytest

E2E：
npx playwright test
