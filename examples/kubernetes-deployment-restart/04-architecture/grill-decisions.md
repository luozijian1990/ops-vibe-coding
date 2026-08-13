# Grill Decisions

> Example Artifact. These are illustrative human-confirmed constraints for teaching,
> not decisions made for a real deployment.

## Input

- [Stack Selection Draft](stack-selection.draft.md)

## Decisions

| Constraint | Example decision | Architecture effect |
|---|---|---|
| Scale | Internal tool with low request volume | No queue, cache or horizontal scaling requirement |
| Deployment | Runs in the existing Kubernetes platform namespace | Reuse current deployment and network controls |
| Authentication | Existing company SSO supplies user identity | Backend trusts only verified server-side identity |
| Authorization | Backend maps identity to namespace-scoped RBAC | Frontend visibility is not authorization evidence |
| Restart operation | Short synchronous Kubernetes API mutation | One backend request with an explicit timeout |
| Audit | Every allowed, denied, successful and failed attempt must persist | Reuse the existing PostgreSQL audit store |
| Retry | Mutation is never automatically retried | Timeout or ambiguous result is returned as Failed |
| Duplicate execution | Disable repeated UI submission and reject the same in-flight request | Add an operation identifier and in-flight guard |
| Credentials | Service account can patch Deployments only in allowed namespaces | No cluster-admin credential |
| HA | Not required for v0.1 | Accepted because this is a low-volume internal tool |

## Human-controlled Decisions

In a real project, the named owner must approve authentication, authorization,
credential scope, retry behavior, audit retention and accepted availability risk.

## Next

[Architecture Final](stack-selection.md) maps every retained component to these
constraints and removes unsupported candidates.
