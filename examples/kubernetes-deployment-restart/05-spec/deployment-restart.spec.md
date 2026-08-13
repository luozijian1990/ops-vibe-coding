# Deployment Restart Spec

> Example Artifact. This is a teaching contract, not an API offered by this repository.

## Inputs

- [Prototype Approval](../01-prototype/approval.md)
- [React Mock Contract](../02-react-mock/contract.md)
- [Architecture Final](../04-architecture/stack-selection.md)

## Core Behavior

An authenticated user can request a restart for one Deployment in an authorized
namespace after explicit confirmation. The system returns a clear outcome and records
the attempt for audit lookup.

## Authorization

- The backend MUST obtain identity from the verified company SSO integration.
- The backend MUST verify namespace-scoped authorization before calling Kubernetes.
- Hiding or disabling the frontend button MUST NOT replace backend authorization.
- A denied request MUST NOT call Kubernetes and MUST produce an audit record.

## API Contract

```text
POST /api/deployments/{namespace}/{name}/restart
Request header: X-Operation-ID: <unique identifier>

200: restart request completed
403: authenticated user is not authorized for the namespace
404: target Deployment does not exist or is not visible to the user
409: the same operation is already in flight
504: Kubernetes API timed out
```

```text
GET /api/audit?resource=deployment&namespace={namespace}&name={name}

200: audit records visible to the authorized user
403: user cannot view audit records for the namespace
```

## State

```text
Idle → Confirmation → Running → Success
                              ↘ Failed
```

While Running, the UI MUST prevent another submission for the same operation. A page
refresh does not authorize an automatic retry.

## Audit

Each allowed, denied, successful and failed attempt MUST record:

- operation identifier
- user identity
- namespace and Deployment name
- requested time and completed time when known
- outcome and non-secret failure category

Audit records MUST NOT contain SSO tokens, Kubernetes credentials or raw Secret values.

## Failure Behavior

- Kubernetes API timeout MUST surface as Failed and MUST be audited.
- The system MUST NOT automatically retry the restart mutation.
- Audit persistence failure MUST prevent a Success response; the response MUST state
  that the operation result cannot be safely confirmed.
- User-visible errors MUST not expose credentials or internal stack traces.

## Test Seams

- Replaceable authorization evaluator
- Fake Kubernetes client supporting success, denial, not found and timeout
- Replaceable audit repository supporting persistence failure
- Frontend Mock API supporting Running, Success and Failed states

## Acceptance Criteria

1. An authorized user confirms one restart and receives a Success result.
2. A cancelled confirmation sends no mutation request.
3. An unauthorized request returns `403`, never calls Kubernetes and is audited.
4. A Kubernetes timeout returns `504`, is visible as Failed and is not retried.
5. A duplicate in-flight operation returns `409` without a second mutation.
6. Successful and failed attempts are visible through the authorized audit API.
7. Responses and audit records contain no credential or Secret value.

## Out of Scope

Batch restart, rollback, scheduled restart, automatic retry and HA are not part of v0.1.

## Next

The Spec is divided into two user-visible vertical Tickets:
[Restart Result](../06-tickets/ticket-01.md) and
[Audit Record](../06-tickets/ticket-02.md).
