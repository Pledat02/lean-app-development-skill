# Traceable software design

Design is the controlled transformation from requirements into an implementable system structure. It sits after the SRS or acceptance-criteria baseline and before coding.

The requirements baseline remains authoritative for externally observable behavior. Design explains how components, interfaces, data, state, and failure handling satisfy that baseline. When design reveals an impossible, contradictory, or missing requirement, revise the SRS through an explicit decision; never solve the conflict by silently changing behavior in code.

## Determine depth

- **Quick**: record an inline design note naming the affected component, contract, state change, and verification impact. A durable file is optional unless the user asks for one.
- **Standard**: create or update `docs/specs/<feature-slug>/design.md` with boundaries, interfaces, data/state flow, failure behavior, requirement coverage, and vertical implementation slices.
- **Critical**: add trust boundaries, authorization enforcement, sensitive-data flow, concurrency control, idempotency, migration sequencing, rollback/recovery, observability, compatibility, and operational ownership.

Use the repository's established design format and location when present.

## Required design structure

```markdown
# Software Design: <Product or feature>

## Document control
- Design ID: <stable identifier>
- Version: <document version>
- Status: Draft | In Review | Ready for Implementation | Implemented | Superseded
- Requirements baseline: <SRS ID and version or acceptance-criteria reference>
- Owner: <person or team when known>
- Last updated: YYYY-MM-DD

## 1. Design goals and non-goals
## 2. Requirements coverage summary
## 3. Existing-system context
## 4. Proposed architecture and boundaries
## 5. Component responsibilities
## 6. Data model and ownership
## 7. Interfaces and contracts
## 8. State transitions and workflows
## 9. UI states and interaction design
## 10. Failure handling and recovery
## 11. Security, privacy, and trust boundaries
## 12. Performance and operational considerations
## 13. Migration, compatibility, rollout, and rollback
## 14. Testability and verification hooks
## 15. Vertical implementation slices
## 16. Requirement-to-design traceability
## 17. Decisions, alternatives, and open issues
```

Omit or mark sections `N/A — <reason>` when they do not apply. Do not generate diagrams or sections that add no decision value.

## Transform requirements without losing them

For each Must requirement:

1. Identify the component that owns the behavior or invariant.
2. Define inputs, outputs, preconditions, postconditions, and failure behavior.
3. Identify data ownership and the authoritative enforcement point.
4. Describe relevant state transitions and external interfaces.
5. Name the verification seam: unit boundary, API contract, integration point, UI state, or E2E journey.
6. Link the design element back to the original requirement ID.

Do not treat a UI-only check as enforcement for authorization, money, inventory, uniqueness, or another server-owned invariant.

## Requirement-to-design traceability

Maintain a matrix such as:

| Requirement | Design element | Owning component | Contract or invariant | Verification seam | Status |
|---|---|---|---|---|---|
| FR-012 | DES-COMP-003 | BookingService | Create booking atomically | Integration | Covered |
| SEC-004 | DES-TRUST-002 | Booking API | Server verifies resource ownership | API + E2E | Covered |
| ERR-006 | DES-UI-005 | BookingForm | Preserve input and expose retry | Component + E2E | Covered |

Every Must requirement must be `Covered`, `Deferred` with explicit authorization, or `Blocked` by an open decision. A blank or missing row fails the design-readiness check.

## Component and contract design

Break work into related components with one clear owner for each business entity and invariant. Prefer existing module boundaries. For every changed contract, define:

- caller and owner;
- request/input and response/output shape;
- validation and authorization;
- transaction or consistency boundary;
- errors and retry semantics;
- compatibility and versioning impact;
- logs or metrics needed for a concrete operational question.

Avoid introducing services, queues, caches, event buses, or abstractions without an observed requirement and an operational owner.

## UI design

Translate UI requirements into explicit states and transitions rather than only static screens:

- entry, loading, empty, validation, submitting, success, error, degraded, expired-session, and retry states;
- keyboard/focus behavior and accessible names;
- responsive reflow, overflow, long content, and supported viewports;
- preservation or reset of user input after failure;
- frontend-backend contracts and server-authoritative validation;
- E2E-observable outcomes for critical journeys.

Reference existing design-system components and design artifacts instead of duplicating their visual specification.

## Design decisions

Record only consequential choices. For each decision include context, selected option, alternatives considered, trade-offs, reversibility, and affected requirement IDs. Do not label a design `Ready for Implementation` while a Critical requirement depends on an unresolved decision.

## Vertical implementation slices

Split coding into thin end-to-end slices that each satisfy traceable requirements across the necessary layers. Each slice should name:

- requirement IDs and design elements;
- files or modules likely to change;
- data/API/UI effects;
- focused developer checks;
- independent verification expected after handoff.

The implementation may refine internal details while coding, but any changed boundary, contract, invariant, or requirement mapping must update the design artifact.

## Design-readiness check

Before coding Standard or Critical work, verify:

- the SRS version or acceptance baseline is named;
- every Must requirement has an owning design element and verification seam;
- component ownership and system boundaries are unambiguous;
- data, interface, state, error, and recovery behavior are defined where relevant;
- security enforcement occurs at a trusted boundary;
- UI states include relevant edge and failure behavior;
- migration and rollback are credible for risky data or contract changes;
- open decisions do not block a Critical requirement;
- implementation slices preserve requirement traceability.

If the user requested design only, deliver the design and stop. Do not infer authorization to implement it.
