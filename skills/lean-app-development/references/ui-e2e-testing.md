# UI and end-to-end testing

Use this reference when user-visible interface behavior or a complete user journey changes. Prefer the repository's existing browser framework, fixtures, page objects, accessibility tooling, and commands. Do not introduce Playwright, Cypress, or another framework merely because it is familiar; add a new framework only when the project lacks a viable option and the maintenance cost is justified.

## Select the test layer

- Use component or interaction tests for dense input combinations, local rendering states, and fast accessibility feedback.
- Use API or integration tests for business rules, authorization, data integrity, retries, and combinatorial edge cases that do not require a browser.
- Use browser E2E for critical user journeys, frontend-backend integration, routing, session behavior, and failures whose UI response matters.
- Use visual comparison only when layout or styling regression is a material risk and the project has a stable baseline process.

Do not force every edge case through E2E. Keep the browser suite small and high-value; place combinatorial coverage at the lowest layer that credibly proves the behavior.

## Edge-case-first browser plan

Start from changed state transitions and failure cost. Prioritize relevant cases such as:

1. unauthorized, expired-session, and authenticated-but-forbidden behavior;
2. empty, missing, malformed, minimum, maximum, and over-limit input;
3. duplicate submit, rapid repeat action, refresh, back/forward navigation, and retry;
4. server validation, network error, timeout, partial response, and recovery;
5. stale state and concurrent changes visible to the user;
6. loading, empty, error, disabled, success, and degraded states;
7. narrow and wide viewport reflow, text overflow, zoom, and touch targets;
8. keyboard-only operation, focus order, focus restoration, accessible names, and associated error messages;
9. timezone, locale, date boundary, currency, Unicode, and long-content behavior;
10. deep links, redirects, browser reload, and persisted state.

After the relevant adverse paths, run one representative happy path for each critical changed journey. Add more happy-path variants only when they cover distinct roles, state transitions, integrations, or browser behavior.

## UI inspection

Inspect the rendered result at the project's supported viewports and interaction states. Check:

- content hierarchy and whether primary actions remain discoverable;
- alignment, overlap, clipping, scroll traps, and responsive reflow;
- loading indicators, skeletons, empty states, validation, errors, and recovery actions;
- focus visibility, keyboard reachability, modal focus containment, and focus return;
- accessible role/name/state semantics and form-error association;
- console errors, failed network requests, unexpected redirects, and hydration errors;
- preservation of user-entered data after recoverable failure.

Use screenshots, traces, video, console output, and network logs as failure evidence when the tooling supports them. Do not treat a screenshot alone as proof that interaction behavior works.

## Stable test design

- Select elements by accessible role, label, or stable test contract rather than CSS layout or generated classes.
- Control time, randomness, network responses, and test data where practical.
- Give each test isolated data and clean up state without depending on execution order.
- Wait for observable application state rather than fixed sleeps.
- Keep secrets and production customer data out of fixtures, recordings, screenshots, and traces.
- Quarantine is temporary and documented; do not hide a regression by retrying indefinitely or weakening assertions.

## Evidence report

Report browser/framework, viewport, relevant environment, commands, scenarios, result, and artifact paths. For a failure include concise reproduction steps, expected versus actual behavior, and the strongest available evidence. Separate application defects, test defects, environment failures, and pre-existing issues.

