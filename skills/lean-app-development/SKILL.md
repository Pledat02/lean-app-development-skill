---
name: lean-app-development
description: Write ISO/IEC/IEEE 29148-aligned SRS specifications, translate requirements into traceable software designs, and build, change, fix, or review small production applications with independent verification. Use for products serving roughly 50-1000 users when a lean alternative to enterprise delivery workflows is appropriate; do not use for hyperscale distributed platforms or work that requires a formal regulated lifecycle.
---

# Lean App Development

Deliver the smallest reliable change that satisfies the user's goal. Treat user count as a capacity hint, not a risk score: authentication, authorization, payments, migrations, concurrency, personal data, and production operations remain high-risk even at low traffic.

## Start with project truth

Before non-trivial work:

1. Read the repository's `AGENTS.md` and any linked project memory, user rules, architecture notes, or domain skill.
2. Inspect the working tree and current implementation. Preserve unrelated user changes.
3. Prefer existing conventions, dependencies, deployment targets, and module boundaries over introducing parallel abstractions.
4. Identify missing authority before destructive, irreversible, externally visible, or production-changing actions.

Project instructions override this skill. Never place credentials or production secrets in code, documentation, prompts, logs, or memory files.

## Choose work depth by risk

- **Quick**: documentation, copy, styling, isolated configuration, or a narrow low-risk bug. Inspect, make the focused change, run proportionate checks, and hand off.
- **Standard**: a normal feature or refactor spanning multiple files, an API boundary, or ordinary persistent data. Establish acceptance criteria, perform a short impact/design pass, implement a vertical slice, and verify it.
- **Critical**: authentication, authorization, payments, destructive migrations, concurrency, personal data, security, irreversible operations, or production deployment. Make risks and rollback explicit, obtain approval for material irreversible choices, and run broader verification.

When the track is unclear, read [references/workflow.md](references/workflow.md). Escalate depth because of concrete impact or uncertainty, not ceremony.

## Use four lenses across two contexts

Apply Product, Architecture, and Development in the **Development context**:

- **Product**: define the outcome, in/out scope, acceptance criteria, and user-visible value.
- **Architecture**: expose boundaries, ownership, data flow, failure behavior, and consequential tradeoffs.
- **Development**: scan before editing, follow repository conventions, and keep the change cohesive and readable.

Apply Quality in a separate **Verification context** for every source-code or deployable-configuration change. The verifier receives the user request or SRS, acceptance criteria, repository snapshot or diff, authoritative commands, and environment constraints. Do not pass the developer's private reasoning, confidence, or conclusions; the verifier must inspect the implementation independently.

The Development context may run focused checks before handoff, but it cannot certify completion. The Verification context owns acceptance testing, UI inspection, and end-to-end testing. It prioritizes rejection, boundary, failure, recovery, authorization, and concurrency cases over additional happy-path coverage while retaining at least one representative happy-path smoke test.

Read [references/independent-verification.md](references/independent-verification.md) for the handoff contract. If a separate context is unavailable, report that independent verification was not performed and do not present the change as fully verified.

## Specification mode

When the user asks for a spec, SRS, requirements document, technical specification, or a durable contract before implementation, read [references/specification.md](references/specification.md). Use its ISO/IEC/IEEE 29148-aligned SRS structure unless the repository defines another required standard.

- Write the spec before code when the request is spec-only, or when a Standard/Critical feature needs decisions preserved across sessions.
- Use the repository's established spec location. Otherwise default to `docs/specs/<feature-slug>/srs.md`.
- Keep Quick work inline unless the user explicitly asks for a written spec.
- Ground every requirement in the user request, project rules, existing behavior, or a necessary safety invariant.
- Present unresolved product or architecture choices explicitly; do not invent them to make the document look complete.
- For a spec-only request, stop after delivering the document and review findings. Do not infer authorization to implement it.

## Design layer between requirements and code

For Standard and Critical implementation work, read [references/design.md](references/design.md) after the SRS or acceptance criteria are stable and before coding. Create or update `docs/specs/<feature-slug>/design.md` unless the repository has an established design location.

The SRS defines **what and why**; Design defines **how the system will satisfy it**. Preserve SRS requirement IDs in a requirement-to-design traceability matrix. Design may split requirements into components, interfaces, data models, state transitions, UI states, failure handling, and implementation slices, but it must not silently weaken, replace, or invent product requirements. If design exposes a conflict or missing decision, update the SRS or record an open decision before coding.

Quick code changes may use a short inline design note. Standard and Critical work require a durable design artifact and a design-readiness check before implementation.

## Working contract

1. Restate the desired outcome internally and surface only assumptions that materially affect it.
2. Ask concise questions only when the answer would change scope, architecture, safety, cost, or external behavior. Otherwise proceed with a reasonable stated assumption.
3. For existing systems, identify affected files, consumers, tests, contracts, data, and deployment surfaces before editing.
4. Establish the SRS or acceptance criteria as the requirements baseline. Preserve stable requirement IDs for Standard and Critical work.
5. Translate the baseline into a traceable design before coding. Check component ownership, interfaces, data and state flow, failure behavior, security, UI states, testability, and rollout implications.
6. Implement the smallest cohesive vertical slice from the approved design. Avoid speculative infrastructure and unrelated cleanup.
7. Run focused developer checks, then hand the SRS, design, and implementation artifact to a separate Verification context. Read [references/verification.md](references/verification.md) whenever source code or deployable configuration changes.
8. For user-interface behavior or a changed user journey, also read [references/ui-e2e-testing.md](references/ui-e2e-testing.md). The verifier tests high-risk edge and failure paths before expanding happy-path coverage.
9. Return actionable findings to the Development context. After fixes, the separate Verification context reruns affected checks and any necessary regression suite.
10. Hand off with requirement and design traceability, important decisions, independent verification evidence, and remaining risks or follow-ups.

For backend, data, authentication, payment, infrastructure, or cross-cutting brownfield work, read [references/engineering-guardrails.md](references/engineering-guardrails.md) before implementation.

## Human control

Require explicit confirmation immediately before destructive data operations, production deployment, paid resource creation, public publishing, or an irreversible architecture choice. Approval for the task does not authorize a materially different external action.

When the user requests changes to a proposed result, choose the lightest appropriate response:

- **Keep** when evidence shows the current result already satisfies the request.
- **Modify** when a focused revision is sufficient.
- **Redo** only when the underlying requirements or design are invalid.

Do not create workflow state, audit logs, stage diaries, or extra artifacts unless the repository or user explicitly requires them.
