# Verdict: new policy: playwright only if necessary

id: prior
reviewer: reviewer
date: 2026-10-11

<!-- Produced by @reviewer and/or @security after implementation. -->

## Review

TASK-1: PASS
notes: `policies/playwright-only-if-necessary.md` (new, 7 lines) exists and
carries the brief's rule: prefer static analysis / reading code / lighter tests
over browser automation, reaching for Playwright (or `shot`) only when those
cannot answer the question, with a one-line why ("Driving a browser is slow and
costly..."). It follows the established format of the four sibling policies
(`ask-before-adding-dependencies.md`, `keep-it-committed.md`,
`no-debug-leftovers.md`, `secrets-hygiene.md`): plain markdown, no frontmatter,
filename-as-identity, one-line heading + rule + why. Framed around the universal
"cheapest sufficient verification first" principle with Playwright named only as
the motivating example, per the analyst's resolution — so it is not a purely
tool-named situational rule. Tool-neutral, no vendors involved.

TASK-2: PASS
notes: The audit was performed and fully recorded in `implementation.md`. It
checks the file against the `medical_policies` criteria and every always-on /
on-demand catalog source. I spot-checked the cited references and they are
accurate: `prompts/meta.md:12-13` ("Prefer concrete verification over
assumption...") verifies; `agents/developer.md:107-129` ("Verifying rendered
work" / `shot`) verifies; `agents/tester.md:96` (`webapp-testing` / Playwright)
and `tester.md:50` ("not everything needs a browser") verify. The reasoning that
the policy adds scope (an ordering/rationing constraint meta.md and the
role-scoped tester bullet do not carry) rather than restating the
`webapp-testing`/`frontend-design` skills is sound. The "complements
`agents/developer.md`" conclusion holds: for rendered UI, static analysis
genuinely cannot answer pixel/contrast/overflow questions, so a `shot` render
stays the correct call; the policy only governs *when* a browser is warranted.
Keeping the wording as delivered (no trim) is a defensible outcome of TASK-2.

Scope: clean. `git diff main...HEAD --name-status` (base `main`; `.manigot/`
config is no longer tracked, so the `main` fallback applies) shows exactly the
new `policies/playwright-only-if-necessary.md` plus this job's own artifacts
(brief/tasks/implementation/verdict/session.log/tokens.json). No changes to
`agents/`, `skills/`, `mcp/`, `prompts/`, `docs/` content or the generated mount
mirrors. `policies/` contains exactly the five `.md` files — no README, hidden
files, or symlinks. The `session.log`/`tokens.json` additions are framework
bookkeeping, not developer edits — not a finding.

## Security

No security findings. A single additive markdown policy file; no code path, no
secrets, no new dependencies.

## Overall

APPROVED

Both tasks are satisfied and the delivered policy matches the brief's rule and
the established policy format. Nothing must change before merge.
