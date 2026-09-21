# Proportional verification

Verification should demonstrate the requested behavior and protect the affected risk. It is evidence, not ceremony or a coverage-number contest.

For source-code or deployable-configuration changes, verification runs in a separate context from development. Follow [independent-verification.md](independent-verification.md). For UI behavior or changed user journeys, also follow [ui-e2e-testing.md](ui-e2e-testing.md).

## General method

1. Start a separate Verification context with the SRS or acceptance criteria, traceable design, implementation artifact, authoritative commands, and environment constraints.
2. Identify the repository's generated-file rules and establish a pre-change baseline when attribution matters.
3. Map each requirement and design element to a test, static check, build, browser interaction, inspection, or explicit manual check; flag missing traceability before treating the change as complete.
4. Prioritize relevant rejection, boundary, failure, recovery, authorization, retry, concurrency, and data-integrity cases.
5. Run at least one representative happy path for each changed critical journey, then the broader suite justified by the blast radius.
6. Return confirmed regressions to Development; after fixes, rerun failed checks and affected regression coverage in the separate Verification context.
7. Record commands, pass/fail results, evidence, pre-existing failures, and anything not verified.

Never claim a check passed when it was not run. Do not silently weaken existing tests or quality gates to obtain a green result.

## Verification matrix

| Change surface | Minimum useful evidence | Increase depth when |
|---|---|---|
| Documentation or copy | Link/format inspection | Generated docs, contracts, or user instructions may become misleading |
| Frontend UI | Type/lint/build, rendered UI inspection, and focused interaction test | Routing, forms, accessibility, responsive behavior, failure states, or shared state changes |
| Critical user journey | One browser E2E smoke plus risk-based adverse paths at the appropriate test layer | Auth, payment, persistence, multi-step workflow, external integration, or recovery changes |
| Backend logic | Unit tests plus package type/lint/build checks | Transactions, authorization, integrations, or cross-module behavior changes |
| API contract | DTO/schema validation and request/response tests | Public clients, compatibility, authentication, or generated clients are affected |
| Database | Migration validation and integration tests | Existing data, locks, destructive operations, uniqueness, or rollback is involved |
| Defect fix | A regression test that fails before and passes after when practical | The root cause is cross-cutting or previously escaped multiple layers |
| Authentication/authorization | Positive and negative server-side tests | Roles, ownership, token verification, or sensitive resources change |
| Payment/callback | Authenticity, idempotency, retry, duplicate, and out-of-order tests | Money movement or reconciliation semantics change |
| Concurrent state change | Competing-request integration test and database invariant inspection | Duplicate booking, overselling, counters, inventory, or uniqueness is possible |
| Deployment configuration | Config validation, build, and rollback/readiness review | Production resources, migrations, DNS, credentials, or traffic shifting changes |

## Test quality

- Test observable requirements rather than internal implementation details.
- Favor many fast unit tests, fewer integration tests, and a small number of high-value end-to-end tests.
- Spend more test-design effort on relevant edge, rejection, failure, and recovery behavior than on repeated happy-path variants.
- Retain at least one representative happy path for each critical journey; edge-case priority does not mean success paths may be omitted.
- Keep tests independent of execution order and shared mutable state.
- Include rejection and boundary cases for state-changing behavior.
- Use representative fixtures without production secrets or private customer data.
- Treat coverage percentages as diagnostic signals, not targets by themselves.

## Stop conditions

Do not declare the change complete when:

- A required check is failing because of the change.
- A Critical invariant has no credible verification.
- Independent verification was required but no separate context was available.
- The runtime or dependency needed for required checks is unavailable and no equivalent check exists.
- Production behavior is claimed from mocks or local-only evidence.
- Deployment, destructive migration, or external publication still requires user authorization.

Hand off partial progress plainly when an external dependency or missing authority prevents completion.
