# Engineering guardrails for small production apps

Optimize for understandable operations and reversible change. A product serving 50-1000 users usually benefits more from simplicity than from distributed-system machinery, but modest traffic does not reduce correctness or security requirements.

## Architecture defaults

- Prefer a modular monolith with explicit module ownership.
- Prefer one primary transactional database and one authoritative owner for each entity.
- Prefer synchronous request/response flows until latency, reliability, or workflow duration creates a demonstrated need for asynchronous processing.
- Prefer managed authentication, database, storage, email, and hosting services when they meet the product's constraints.
- Keep contracts explicit at module, API, and database boundaries.
- Prefer reversible decisions. Give extra scrutiny to public contracts, destructive schema changes, identity models, and data ownership.

Do not introduce microservices, Kubernetes, event buses, queues, Redis, WebSockets, CQRS, event sourcing, or multi-region architecture without a concrete use case and operational owner. Existing justified infrastructure is not removed merely because this skill prefers simpler defaults.

## Brownfield safeguards

Before changing existing code:

1. List the files or modules likely to change.
2. Identify callers, imports, routes, contracts, tests, configuration, migrations, and generated outputs connected to them.
3. Classify the blast radius as narrow, multi-component, or cross-cutting.
4. Check the current verification baseline when comparing before/after results will prevent false attribution.
5. Preserve unrelated worktree changes and generated-file ownership rules.

Use a formal impact summary only for multi-component or cross-cutting changes. Low-risk focused edits need only enough inspection to avoid collateral damage.

## Data and concurrency

- Enforce critical invariants in the database as well as application logic.
- Wrap multi-write business operations in a transaction.
- Use uniqueness, exclusion, or equivalent constraints to prevent invalid concurrent states.
- Make externally retried state-changing operations idempotent.
- Store money in an exact integer or decimal representation appropriate to the currency; never use binary floating point.
- Store absolute instants consistently and convert business timezones at system boundaries.
- Review migrations for forward compatibility, existing data, locks, rollback, and deployment ordering.
- Do not run destructive production migrations without explicit authorization and a recovery plan.

## Authentication and authorization

- Let a trusted identity provider own passwords, session issuance, and token refresh unless the product explicitly requires otherwise.
- Authenticate identity at a trusted boundary and authorize every protected operation on the server.
- Derive security roles and ownership from trusted data, not user-editable profile metadata.
- Apply least privilege to service credentials and database access.
- Never log tokens, passwords, cookies, OTPs, connection strings, payment secrets, or sensitive payloads.

## Payments and external callbacks

- Verify callback authenticity using the provider's documented mechanism.
- Make callback processing idempotent and safe for duplicate or out-of-order delivery.
- Keep provider status separate from internal business status and define the mapping explicitly.
- Reconcile uncertain outcomes instead of assuming a timeout means failure.
- Treat client-side payment success as advisory until the server verifies it.

## Deployment and rollback

- Separate implementation completion from production deployment approval.
- Identify required environment variables without writing their secret values into tracked files.
- Prefer backward-compatible schema and API changes when deployments may overlap.
- Define the rollback or forward-fix strategy for Critical changes.
- Add logs, metrics, or alerts only where they support a concrete operational question.

## Decision test

Before adding complexity, answer:

1. What observed problem does this solve now?
2. Why cannot the existing stack solve it adequately?
3. Who will operate it when it fails?
4. What is the simpler alternative and its actual downside?
5. How can the decision be reversed?

If the answers are weak, keep the architecture simpler.
