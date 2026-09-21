# SRS specification mode

Create a Software Requirements Specification aligned with the requirements-engineering principles of ISO/IEC/IEEE 29148:2018. This skill does not claim formal certification or reproduce the standard. It produces a practical SRS whose requirements are necessary, unambiguous, feasible, singular, verifiable, and traceable.

Normative reference profile: [ISO/IEC/IEEE 29148:2018 — Requirements engineering](https://www.iso.org/standard/72089.html). Check the project's mandated edition before claiming standards conformance.

Use the project's established format and location when one exists; otherwise write `docs/specs/<feature-slug>/srs.md`. If a repository mandates a different requirements standard, follow it and record the variance.

## Determine depth

- **Quick**: create an SRS only when explicitly requested. Keep the required metadata, scope, actors, functional requirements, constraints, acceptance criteria, and traceability; mark inapplicable sections instead of inventing content.
- **Standard**: include the complete structure below with user flows, interfaces, data, failure behavior, non-functional requirements, verification, and traceability.
- **Critical**: add trust boundaries, authorization, sensitive data, concurrency, idempotency, migration, rollback, observability, operational recovery, and deployment compatibility where relevant.

Do not fill sections with generic prose. Use `N/A — <reason>` when a section genuinely does not apply. Record unresolved material as a named open decision with an owner when known and explain which requirements depend on it.

## Required SRS structure

```markdown
# Software Requirements Specification: <Product or feature>

## Document control
- SRS ID: <stable identifier>
- Version: <semantic document version>
- Status: Draft | In Review | Approved | Implemented | Superseded
- Owner: <person or team when known>
- Reviewers: <roles or people when known>
- Last updated: YYYY-MM-DD
- Related artifacts: <issue, design, API contract, decision record>

## 1. Introduction
### 1.1 Purpose
### 1.2 Scope
### 1.3 Intended audience
### 1.4 Definitions and abbreviations
### 1.5 References

## 2. Overall description
### 2.1 Product perspective and system boundary
### 2.2 Product functions
### 2.3 User classes and permissions
### 2.4 Operating environment
### 2.5 Constraints
### 2.6 Assumptions and dependencies

## 3. External interface requirements
### 3.1 User interfaces
### 3.2 Software and API interfaces
### 3.3 Data interfaces
### 3.4 Communications interfaces

## 4. Specific requirements
### 4.1 Functional requirements
### 4.2 Business rules
### 4.3 Data requirements
### 4.4 Non-functional requirements
### 4.5 Security and privacy requirements
### 4.6 Failure, recovery, and edge-case requirements

## 5. User flows and state transitions
## 6. Verification and acceptance
## 7. Requirements traceability matrix
## 8. Design handoff constraints
## 9. Rollout, migration, and rollback
## 10. Open decisions
## Appendices
```

The document-control status is metadata, not workflow state. Never claim `Approved` or `Implemented` without evidence.

## Requirement format

Give each requirement a stable, unique identifier:

- `FR-###`: functional behavior.
- `BR-###`: business rule or invariant.
- `DR-###`: data requirement.
- `IR-###`: interface requirement.
- `NFR-###`: measurable quality attribute.
- `SEC-###`: security or privacy requirement.
- `ERR-###`: rejection, failure, recovery, or edge-case behavior.

Write one requirement per statement. Use `shall` for mandatory behavior and avoid combining multiple obligations with `and` unless they are indivisible.

```markdown
### FR-012 — Submit a booking
- Statement: The system shall create a booking only when the requested slot remains available at commit time.
- Rationale: Prevent duplicate allocation during concurrent requests.
- Source: User request / BR-004
- Priority: Must
- Preconditions: Authenticated user; valid facility and slot.
- Inputs: facility_id, slot_id, request_id
- Expected result: One committed booking with a stable identifier.
- Failure behavior: Return the defined conflict response without creating a partial booking.
- Verification: Integration test with competing requests.
- Traces to: AC-007, TC-021
```

Do not prescribe implementation unless it is an explicit constraint. Replace vague terms such as “fast”, “secure”, “user-friendly”, “normally”, or “as appropriate” with measurable behavior or an open decision.

## Edge-case-first requirements

For every state-changing capability, define the primary success requirement and then give greater analytical depth to relevant adverse behavior:

- empty, missing, malformed, minimum, maximum, and over-limit inputs;
- unauthorized and authenticated-but-forbidden actors;
- duplicate submission, retry, timeout, and idempotency;
- stale state, concurrent requests, ordering, and race conditions;
- dependency failure, partial failure, offline behavior, and recovery;
- empty, loading, error, and degraded UI states;
- timezone, locale, money, encoding, and date-boundary behavior;
- accessibility and keyboard-only operation for user interfaces.

Do not invent irrelevant edge cases to increase document length. Select them from the actual data, state transitions, trust boundaries, integrations, and failure cost.

## User interface requirements

Describe observable states and behavior rather than visual taste. Include when relevant:

- viewport and supported-browser constraints;
- loading, empty, error, disabled, validation, success, and retry states;
- focus order, keyboard operation, accessible names, contrast, and error association;
- preservation or reset of user input after failure;
- responsive reflow and content overflow;
- browser navigation, refresh, deep link, and session-expiry behavior.

Link critical UI behavior to E2E acceptance criteria. Keep detailed brand and visual design in the project's design system or design artifact rather than duplicating it in the SRS.

## Verification and acceptance

Every Must requirement needs a verification method: inspection, static analysis, unit, integration, contract, UI interaction, end-to-end, performance, security, or explicit manual evaluation. Map acceptance criteria to requirements and give rejection, boundary, or recovery criteria more coverage than duplicate happy-path variants.

Write acceptance criteria as observable Given/When/Then statements or equivalently precise assertions. Retain at least one representative happy path for each critical user journey.

## Traceability matrix

Include at least:

| Requirement | Source | Design element | Acceptance criteria | Verification method | Test or evidence | Status |
|---|---|---|---|---|---|---|
| FR-001 | User request | Planned | AC-001 | E2E | TC-E2E-001 | Planned |
| ERR-003 | Risk analysis | Planned | AC-006 | Integration | TC-INT-014 | Planned |

Do not fabricate test IDs or completion status. Use `Planned` and leave evidence blank until tests exist and run.

## Design handoff

The approved SRS is the behavioral baseline for [design](design.md). Design must preserve requirement IDs and fill the `Design element` traceability column without rewriting the requirement statement. When design exposes a contradiction, infeasible constraint, or missing decision, revise and version the SRS explicitly before implementation.

## Review checklist

Before delivering the SRS, verify:

- The system boundary, actors, permissions, assumptions, and external interfaces are explicit.
- Each requirement is singular, necessary, feasible, unambiguous, implementation-independent where possible, and verifiable.
- IDs are unique and traceability has no orphan Must requirement.
- The design handoff identifies which requirements need component, data, interface, UI-state, security, migration, or recovery decisions.
- Conflicts with current behavior or architecture are called out rather than silently normalized.
- State-changing behavior covers validation, authorization, retry, duplicate, failure, and recovery where relevant.
- Money and time use exact representations and explicit business timezone semantics where relevant.
- UI requirements cover meaningful loading, empty, error, responsive, keyboard, and accessibility behavior.
- Open decisions identify their impact and who should decide when known.
- No credentials, private customer data, or unsupported production claims appear.

If the user requested only an SRS, deliver it and stop. Do not infer authorization to implement it.
