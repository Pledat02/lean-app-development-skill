---
name: lean-app-development
description: Write specifications and build, change, fix, or review small production applications with risk-based requirements, architecture, implementation, and verification. Use for products serving roughly 50-1000 users when a lean alternative to enterprise delivery workflows is appropriate; do not use for hyperscale distributed platforms or work that requires a formal regulated lifecycle.
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

## Use four lenses in one skill

Apply these perspectives without requiring separate agents:

- **Product**: define the outcome, in/out scope, acceptance criteria, and user-visible value.
- **Architecture**: expose boundaries, ownership, data flow, failure behavior, and consequential tradeoffs.
- **Development**: scan before editing, follow repository conventions, and keep the change cohesive and readable.
- **Quality**: map checks to acceptance criteria and risk; distinguish pre-existing failures from regressions.

Use repository-provided specialist or tester agents when project instructions require them. Do not create a multi-agent process merely because the roles exist.

## Specification mode

When the user asks for a spec, requirements document, technical specification, or a durable contract before implementation, read [references/specification.md](references/specification.md).

- Write the spec before code when the request is spec-only, or when a Standard/Critical feature needs decisions preserved across sessions.
- Use the repository's established spec location. Otherwise default to `docs/specs/<feature-slug>/spec.md`.
- Keep Quick work inline unless the user explicitly asks for a written spec.
- Ground every requirement in the user request, project rules, existing behavior, or a necessary safety invariant.
- Present unresolved product or architecture choices explicitly; do not invent them to make the document look complete.
- For a spec-only request, stop after delivering the document and review findings. Do not infer authorization to implement it.

## Working contract

1. Restate the desired outcome internally and surface only assumptions that materially affect it.
2. Ask concise questions only when the answer would change scope, architecture, safety, cost, or external behavior. Otherwise proceed with a reasonable stated assumption.
3. For existing systems, identify affected files, consumers, tests, contracts, data, and deployment surfaces before editing.
4. Plan at the smallest useful level. A short change needs no formal design artifact; a risky cross-cutting change needs explicit decisions and rollback. Create a durable spec only under Specification mode.
5. Implement the smallest cohesive vertical slice. Avoid speculative infrastructure and unrelated cleanup.
6. Verify in proportion to risk and fix confirmed regressions within scope. Read [references/verification.md](references/verification.md) whenever source code or deployable configuration changes.
7. Hand off with outcome, important decisions, verification evidence, and remaining risks or follow-ups.

For backend, data, authentication, payment, infrastructure, or cross-cutting brownfield work, read [references/engineering-guardrails.md](references/engineering-guardrails.md) before implementation.

## Human control

Require explicit confirmation immediately before destructive data operations, production deployment, paid resource creation, public publishing, or an irreversible architecture choice. Approval for the task does not authorize a materially different external action.

When the user requests changes to a proposed result, choose the lightest appropriate response:

- **Keep** when evidence shows the current result already satisfies the request.
- **Modify** when a focused revision is sufficient.
- **Redo** only when the underlying requirements or design are invalid.

Do not create workflow state, audit logs, stage diaries, or extra artifacts unless the repository or user explicitly requires them.
