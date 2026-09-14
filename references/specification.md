# Specification mode

Create a specification that is precise enough to guide implementation and verification without becoming an enterprise requirements package. Use the project's established format and location when one exists; otherwise write `docs/specs/<feature-slug>/spec.md`.

## Determine depth

- **Quick**: do not create a spec unless the user explicitly requests one. Keep it to the outcome, constraints, and acceptance criteria.
- **Standard**: cover user behavior, business rules, affected boundaries, data/API impact, failure cases, and verification.
- **Critical**: add trust boundaries, authorization, sensitive data, concurrency, idempotency, migration, rollback, observability, and deployment compatibility where relevant.

Omit sections that genuinely do not apply instead of filling them with generic prose. Mark unresolved material as an open decision rather than inventing an answer.

## Required structure

```markdown
# <Feature name>

## Status
- State: Draft | Approved | Implemented | Superseded
- Owner: <person or team when known>
- Last updated: YYYY-MM-DD

## Problem and outcome
## In scope
## Out of scope
## Users and permissions
## User flow
## Functional requirements
## Business rules
## Data and API impact
## Failure and edge cases
## Non-functional requirements
## Acceptance criteria
## Verification strategy
## Rollout and rollback
## Open decisions
```

The Status block is document metadata, not workflow state. Do not claim Approved or Implemented without evidence.

## Writing rules

### Problem and outcome

State the current problem, who experiences it, and the observable outcome. Avoid describing implementation as the problem.

### Scope

Use concrete behavior for In scope. Put tempting adjacent work in Out of scope so implementation does not expand silently.

### Users and permissions

Name actors and the operations each may perform. For protected behavior, state where authorization is enforced. Do not rely only on hidden UI controls.

### User flow

Describe the primary path in numbered steps. Add alternate flows only when they change data, permissions, money, or user recovery.

### Requirements

Use stable IDs when the spec is expected to evolve:

- `FR-01`, `FR-02` for functional requirements.
- `BR-01`, `BR-02` for business rules.
- `NFR-01`, `NFR-02` for non-functional requirements.
- `AC-01`, `AC-02` for acceptance criteria.

Each requirement must be testable and trace to the user's request, a project rule, existing behavior, or a necessary safety invariant.

### Data and API impact

Describe affected entities, ownership, validation, state transitions, API contracts, compatibility, migration, and generated clients. Do not design tables or endpoints that are unnecessary for the behavior.

### Failure and edge cases

Cover validation, missing permissions, retries, duplicate requests, partial external failures, stale state, time boundaries, and concurrency only where relevant.

### Acceptance criteria

Write observable Given/When/Then statements or equivalently precise assertions. Include at least one rejection or recovery criterion for state-changing behavior.

### Verification strategy

Map each important requirement or acceptance criterion to unit, integration, contract, end-to-end, static, or manual evidence. Avoid arbitrary coverage targets.

### Rollout and rollback

Required for Critical changes and optional otherwise. Separate code readiness from authorization to deploy.

## Review checklist

Before delivering the spec, verify:

- The outcome and boundaries are unambiguous.
- Requirements do not contradict project rules or current architecture without calling out the proposed change.
- Permissions and ownership are explicit where applicable.
- State-changing behavior defines failure, retry, and duplicate handling where needed.
- Money and time use exact representations and explicit business timezone semantics where needed.
- Acceptance criteria cover the primary path and material failure paths.
- Open decisions name who must decide and why implementation depends on them.
- No secrets, private customer data, or unsupported production claims appear in the document.

If the user requested only a specification, deliver it and stop. Offer implementation as a separate next action rather than starting it automatically.
