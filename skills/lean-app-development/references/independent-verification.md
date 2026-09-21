# Independent verification contexts

Separate implementation and verification so that the verifier evaluates observable behavior instead of confirming the developer's narrative.

## Context ownership

### Development context

Owns requirements clarification, impact analysis, design, implementation, migrations, and focused developer checks. It may add unit or regression tests while implementing, but it does not decide that the change is independently verified.

### Verification context

Starts as a separate agent or fresh task context after an implementation artifact exists. It owns test design, acceptance verification, UI inspection, E2E execution, regression selection, and the final evidence report. It may add or improve tests when authorized by the task, but returns product-code defects to Development rather than silently repairing them.

Do not share hidden chain-of-thought, abandoned approaches, confidence statements, or the developer's claim that the change is correct. Independence comes from fresh inspection, not from withholding necessary facts.

## Handoff contract

Pass only the material needed to reproduce and assess the change:

- original user outcome and scope;
- approved SRS or acceptance criteria and the traceable design artifact;
- repository, branch, commit, worktree, diff, or immutable snapshot identifier;
- supported environment and authoritative setup/build/test commands;
- external-service constraints and safe test credentials supplied through the environment;
- migrations, feature flags, seed data, and cleanup instructions;
- known pre-existing failures, each backed by baseline evidence;
- explicit areas that could not be verified.

The verifier independently inspects the design, changed code, and tests. The SRS remains authoritative for expected behavior; a conflicting design or developer summary is not evidence that a requirement changed.

## Verification order

1. Confirm the artifact and environment match the handoff.
2. Establish or inspect the relevant baseline when attribution matters.
3. Derive tests from the SRS, design traceability, acceptance criteria, state transitions, trust boundaries, and changed diff.
4. Exercise rejection, boundary, failure, recovery, permission, retry, concurrency, and data-integrity risks first.
5. Run at least one representative happy-path smoke test for each changed critical journey.
6. Check that implementation boundaries, contracts, invariants, and failure handling match the traceable design.
7. Run the broader regression suite justified by the blast radius.
8. Report reproducible findings with expected behavior, actual behavior, evidence, requirement/design IDs, and severity.

Edge-case priority means more reasoning and coverage for high-risk adverse behavior, not skipping the successful user journey.

## Feedback loop

The Verification context returns findings to Development. Development fixes confirmed in-scope defects and provides a new artifact identifier. The same separate Verification context reruns the failed checks plus affected regression coverage. Do not merge the contexts during this loop.

For a test-only defect, the verifier may fix the test when the expected product behavior is already established by the SRS or acceptance criteria. If expected behavior is ambiguous, return an open decision instead of encoding an assumption in a test.

## Fallback and stop conditions

If the environment cannot create a second context, report `Independent verification: not performed`. Current-context checks may still provide useful evidence, but they do not satisfy this skill's independent-verification requirement.

Do not declare the change fully verified when required infrastructure, browser automation, representative data, permissions, or external dependencies are unavailable and no equivalent evidence exists.
