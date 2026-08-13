# Prototype Approval

> Example Artifact. Illustrative approval only; not evidence of a real human review.

## Gate Record

- **Gate**: Human Prototype Review
- **Example Status**: `APPROVED`
- **Reviewer Role**: Human
- **Scope**: Deployment Detail 的单 Deployment Restart Workflow

## Approved Workflow

```text
Deployment Detail
→ Restart
→ Confirmation Dialog
→ Confirm
→ Running
→ Success / Failed
→ Audit Record
```

## Accepted Scope

- Restart one Deployment
- Namespace scoped
- Explicit confirmation required
- Success and failure feedback required
- Audit record required

## Out of Scope

- Batch restart
- Rollback
- Scheduled or cron restart
- Automatic retry

## Decision

示例决定为进入 React + Mock。真实项目必须记录实际 Reviewer、日期、Prototype 版本
和目标 viewport。

## Next

[React Mock Contract](../02-react-mock/contract.md) 消费本文件中获批的 Workflow、范围
与非目标。
