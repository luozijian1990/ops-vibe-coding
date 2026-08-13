# 上下文地图

> 不是所有任务都读取所有文档。按任务类型加载最小上下文。

## 始终读取

AGENTS.md
context/team/
context/harness-framework/

## 架构任务

docs/architecture.md
docs/plans/stack-selection.draft.md

## 后端 Ticket

spec.md
ticket/{ticket-id}.md
docs/security.md

当 Ticket 修改服务契约时，追加读取：

context/project/ops-platform/cluster/restart-api/
contracts/restart-api.openapi.yaml
.service-matrix/dependencies.yaml（仅当多服务项目维护该文件时）

## Kubernetes 变更 Ticket

spec.md
ticket/{ticket-id}.md
docs/security.md
docs/operations.md

## 前端 Ticket

spec.md
ticket/{ticket-id}.md
docs/architecture.md

## E2E

spec.md
docs/plans/e2e-test-plan.md
docs/testing.md
