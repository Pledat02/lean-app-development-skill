# Proportional verification

Verification should demonstrate the requested behavior and protect the affected risk. It is evidence, not ceremony or a coverage-number contest.

## General method

1. Identify the repository's authoritative commands and generated-file rules.
2. When useful, establish a pre-change baseline so existing failures are not blamed on the change.
3. Map each acceptance criterion and important failure mode to a test, static check, build, inspection, or explicit manual check.
4. Run focused checks first for fast feedback, then the required broader suite.
5. Fix regressions introduced by the change. Report unrelated pre-existing failures separately.
6. Record commands, pass/fail results, and anything not verified.

Never claim a check passed when it was not run. Do not silently weaken existing tests or quality gates to obtain a green result.

## Verification matrix

| Change surface | Minimum useful evidence | Increase depth when |
|---|---|---|
| Documentation or copy | Link/format inspection | Generated docs, contracts, or user instructions may become misleading |
| Frontend UI | Type/lint/build plus focused interaction inspection | Routing, forms, accessibility, responsive behavior, or shared state changes |
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
- Keep tests independent of execution order and shared mutable state.
- Include rejection and boundary cases for state-changing behavior.
- Use representative fixtures without production secrets or private customer data.
- Treat coverage percentages as diagnostic signals, not targets by themselves.

## Stop conditions

Do not declare the change complete when:

- A required check is failing because of the change.
- A Critical invariant has no credible verification.
- The runtime or dependency needed for required checks is unavailable and no equivalent check exists.
- Production behavior is claimed from mocks or local-only evidence.
- Deployment, destructive migration, or external publication still requires user authorization.

Hand off partial progress plainly when an external dependency or missing authority prevents completion.
