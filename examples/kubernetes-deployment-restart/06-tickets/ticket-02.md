# Ticket 02 — View the Restart Audit Record

> Example Ticket. It is a vertical user-value slice, not a separate database, API or
> frontend layer ticket.

## User Value

An authorized user can open the audit history for a Deployment and understand who
requested a restart, when it ran and how it ended.

## Inputs

- [Deployment Restart Spec](../05-spec/deployment-restart.spec.md)
- Audit records produced by [Ticket 01](ticket-01.md)

## Scope

- Namespace authorization for audit lookup
- Audit query endpoint and persistence query
- Deployment Detail audit list with outcome and timestamps
- Success result navigation to the corresponding record
- Empty, loading and failed query states
- API, repository and frontend interaction tests

## Non-goals

- Global audit administration
- Export or long-term retention policy changes
- Editing or deleting audit records

## Acceptance

- An authorized user sees records for the selected namespace and Deployment.
- An unauthorized user receives `403` and no audit data.
- Success and Failed outcomes are distinguishable without exposing secrets.
- A successful restart result links to its operation identifier.
- Empty and audit-query failure states are explicit.

## Verification

The Ticket is complete only when backend, frontend and Workflow evidence covers the
same user-visible slice. It must not be closed from an API-only or UI-only result.
