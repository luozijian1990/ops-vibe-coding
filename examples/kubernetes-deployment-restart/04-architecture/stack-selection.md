# Stack Selection Final

> Example Artifact. This final architecture is illustrative and only follows from
> the example Grill decisions in this directory.

## Inputs

- [Stack Selection Draft](stack-selection.draft.md)
- [Grill Decisions](grill-decisions.md)

## Status

`FINAL — example`

## Minimal Architecture

| Component | Responsibility | Evidence |
|---|---|---|
| React frontend | Confirmation, Running, Success, Failed and audit navigation | Approved product Workflow |
| FastAPI backend | Authenticate request, authorize namespace, coordinate mutation and return result | Browser must not access Kubernetes credentials |
| Kubernetes API client | Patch the target Deployment within a bounded timeout | Restart is a Kubernetes mutation |
| Existing PostgreSQL audit store | Persist attempts and outcomes | Grill requires durable audit and confirms existing infrastructure |
| Existing company SSO integration | Provide verified user identity | Grill authentication decision |

## Request Boundary

```text
Browser
→ FastAPI authentication and namespace authorization
→ Kubernetes API patch with timeout
→ Persist audit outcome
→ Return explicit result
```

The backend does not automatically retry restart mutations. An ambiguous timeout is
reported as Failed with an audit record; it is not converted into a claimed success.

## Excluded for v0.1

- Redis, Kafka, Worker and microservice split
- Batch, scheduled and automatic restart
- High availability deployment

## Next

[Deployment Restart Spec](../05-spec/deployment-restart.spec.md) turns these boundaries
into testable behavior and acceptance criteria.
