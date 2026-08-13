# Stack Selection Draft

> Example Artifact. `DRAFT` records capability candidates and open constraints; it
> is not an approved architecture.

## Input

- [React Mock Contract](../02-react-mock/contract.md)
- [Prototype Parity](../03-parity/prototype-parity.md)

## Status

`DRAFT — complete Grill before Finalize`

## Capability Evidence from the UI

- A React page initiates one Deployment restart.
- A backend API is required because browser code must not hold Kubernetes credentials.
- The user needs Running, Success and Failed results.
- Successful and failed attempts require durable audit lookup.

## Minimal Candidates

- **Frontend**: React, already established by the executable product contract.
- **Backend candidate**: FastAPI providing restart and audit endpoints.
- **External dependency**: Kubernetes API.
- **Persistence candidate**: durable audit storage; product behavior does not select a database.

## Open Constraints for Grill

- Deployment environment and expected request volume
- Authentication integration and namespace authorization source
- Kubernetes credential scope
- Restart timeout and duplicate submission behavior
- Audit persistence and retention ownership
- Retry, availability and recovery requirements

## Explicitly Not Introduced

- Redis
- Kafka or another message queue
- Background Worker
- Microservices

No evidence currently requires these components.

## Next

[Grill Decisions](grill-decisions.md) must resolve architecture-changing constraints
before this Draft can become Final.
