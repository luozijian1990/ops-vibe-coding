# TDD Evidence

> Example Artifact. `NOT_EXECUTED`: the commands, failures and results below illustrate
> what a real implementation record should contain. This repository includes no app.

## Scope

- [Ticket 01](../06-tickets/ticket-01.md)
- [Ticket 02](../06-tickets/ticket-02.md)

## RED

Example failing tests are written before implementation:

```text
test_authorized_restart_writes_audit
test_unauthorized_restart_never_calls_kubernetes
test_timeout_is_not_retried
test_duplicate_operation_is_rejected
test_authorized_user_can_view_restart_audit
restart-dialog.spec: cancel sends no request
restart-dialog.spec: running prevents duplicate submission
```

Real evidence must include the exact command and failure output showing that each test
failed for the intended missing behavior, not because of an unrelated environment error.

## GREEN

The smallest implementation should satisfy the Spec using replaceable authorization,
Kubernetes and audit boundaries. A real record would attach test output to the exact
commit under review.

## REFACTOR

Refactoring may remove duplication or clarify boundaries without adding Redis, Kafka,
Workers, microservices or automatic mutation retry.

## VERIFY

Example command categories:

```text
backend unit and integration tests
frontend component and interaction tests
frontend build
lint and static checks
browser Workflow test
```

## Example Status

`NOT_EXECUTED — documentation example`

The next reviews demonstrate their expected structure only. They cannot change this
status into real implementation evidence.
