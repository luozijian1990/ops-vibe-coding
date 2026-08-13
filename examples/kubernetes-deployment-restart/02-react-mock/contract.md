# React Mock Contract

> Example Artifact. This contract describes the executable product behavior a
> real React + Mock implementation would expose; no application is included here.

## Input

- [Prototype Approval](../01-prototype/approval.md)

## Page

`Deployment Detail`

## Interaction

- Primary action: `Restart`
- The action opens a confirmation dialog; it does not mutate immediately.
- The dialog shows Cluster, Namespace and Deployment target values.
- `Confirm` submits the Mock request; `Cancel` returns to Idle.

## States

| State | Product behavior |
|---|---|
| Idle | Restart is available to an authorized user |
| Running | Action is disabled and progress is visible |
| Success | Result is visible and links to the audit record |
| Failed | Error is visible without claiming the Deployment restarted |

## Mock API Shape

```text
POST /api/deployments/{namespace}/{name}/restart
GET  /api/audit?resource=deployment&namespace={namespace}&name={name}
```

The Mock API represents product intent only. It does not establish the real backend
authorization, Kubernetes credential or persistence design.

## Output

This artifact gives [Prototype Parity](../03-parity/prototype-parity.md) a fixed page,
Workflow, state set and API shape to inspect. Architecture treats these as capability
signals, not final component choices.
