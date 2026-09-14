# Risk-based workflow

Use the lightest track that controls the actual risk. The tracks guide judgment; they are not a state machine and do not require persistent workflow files.

## Classify the change

Choose **Quick** when all of these are true:

- The intended result is clear.
- The blast radius is narrow and reversible.
- No security, money, personal data, migration, concurrency, or production action is involved.
- Focused verification can provide meaningful confidence.

Choose **Critical** when any of these applies:

- Authentication, authorization, secrets, payment, or personal data changes.
- Database migrations can lose, reinterpret, or expose data.
- Concurrent requests can create duplicates, oversell inventory, or violate uniqueness.
- The change modifies production infrastructure, public behavior, or a hard-to-reverse contract.
- Requirements are materially ambiguous while failure would be costly.

Use **Standard** for everything between those boundaries.

## Quick track

1. Inspect the target and its direct consumers.
2. Define the expected result in one or two sentences.
3. Make the focused change without unrelated cleanup.
4. Run the narrowest meaningful formatter, lint, build, or focused test.
5. Report the result and any unverified behavior.

Do not manufacture requirements documents or approval gates for routine reversible work.

## Standard track

1. **Frame**: state the user outcome, in-scope behavior, explicit exclusions, constraints, and measurable acceptance criteria.
2. **Discover**: inspect relevant structure, current behavior, dependencies, tests, contracts, data ownership, and deployment configuration.
3. **Design briefly**: describe changed boundaries, data flow, failure behavior, and one or two consequential tradeoffs. Record a durable decision only when future work would otherwise reopen it.
4. **Implement vertically**: complete a user-observable slice across the necessary layers instead of building unused horizontal abstractions.
5. **Verify**: map checks to acceptance criteria and affected risks. Add regression coverage for fixed defects.
6. **Hand off**: summarize behavior, decisions, commands/results, remaining mock or manual behavior, and safe next steps.

## Critical track

Run the Standard track with these additions before implementation:

1. List failure modes and their user/business impact.
2. Make trust boundaries, authorization ownership, sensitive data, and external dependencies explicit.
3. Define migration compatibility, rollback, recovery, and idempotency where applicable.
4. For concurrent state changes, define the transaction boundary and database invariant that prevents invalid states.
5. Present irreversible or materially costly choices for explicit user approval.

After implementation:

1. Exercise happy paths, rejection paths, boundary cases, retries, and concurrency where relevant.
2. Verify authorization at the server-side enforcement point, not only in the UI.
3. Validate migration and rollback behavior against representative data when safe.
4. Separate code readiness from deployment authorization. Never infer permission to deploy.

## Acceptance criteria shape

Prefer a compact set of observable statements:

- Given a relevant starting state, when the user or system performs an action, then the observable outcome is clear.
- Include at least one failure or rejection case for state-changing behavior.
- Include ownership, retry, timezone, money, or concurrency behavior when those concerns apply.
- Every criterion should be verifiable by a test, inspection, or explicit manual check.

Avoid requirements invented from personal preference. Trace each criterion to the user's request, an existing project rule, a domain invariant, or a necessary safety constraint.
