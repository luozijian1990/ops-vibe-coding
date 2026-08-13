# E2E Test Report

> Documentation example. The Workflow and statuses below are illustrative. No browser,
> Kubernetes cluster or production environment was used.

## Gate Record

- **Gate**: E2E
- **Example Outcome**: `PASS`
- **Real Execution Status**: `SKIPPED — no application or test environment`
- **Scope**: One authorized Deployment restart followed by audit lookup
- **Environment**: Documentation example only

## Workflow

```text
Login
→ Deployment Detail
→ Restart
→ Confirm
→ Running
→ Success
→ Audit Record
```

## Example Steps

| Step | Example status | Evidence a real project must attach |
|---|---|---|
| Login through company SSO | `PASS` | Environment, user role and browser record |
| Open an authorized Deployment | `PASS` | Target namespace and page screenshot |
| Open Restart confirmation | `PASS` | Dialog screenshot showing the target |
| Confirm exactly once | `PASS` | Network request and operation identifier |
| Observe Running then Success | `PASS` | State screenshots and sanitized API result |
| Open matching audit record | `PASS` | Audit row tied to the operation identifier |

## Failure Scenarios Expected in Real E2E

| Scenario | Real execution status | Expected behavior |
|---|---|---|
| Unauthorized namespace | `SKIPPED` | `403`, no Kubernetes mutation, denied attempt audited |
| Kubernetes API timeout | `SKIPPED` | Failed result, no automatic retry, timeout audited |
| Duplicate in-flight confirm | `SKIPPED` | `409`, no second mutation |

## Evidence

There is no real screenshot, trace, command output or cluster evidence in this example.
The table defines the evidence slots a project must replace before claiming E2E `PASS`.

## Risk

The example does not validate SSO, backend RBAC, Kubernetes credential scope, audit
persistence, timeout behavior or secret redaction in a running environment.

## Human Decision

Example decision: the illustrative Workflow is complete enough to demonstrate the
Artifact chain. It is not accepted as production test evidence.

## Next

[Human Acceptance](../10-acceptance/human-acceptance.md) shows how a real owner would
consume the product, review and E2E evidence without delegating the final decision to AI.
