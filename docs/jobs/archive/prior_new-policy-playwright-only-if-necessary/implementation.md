# Implementation: new policy: playwright only if necessary

id: prior
developer: developer
date: 2026-10-11

<!-- Produced by @developer after implementation. -->

## Summary

Added one new global catalog policy, `policies/playwright-only-if-necessary.md`,
capturing the brief's rule: browser automation (Playwright, and the `shot`
render path that drives it) is a costly verification path and should be reached
for only when it is genuinely necessary — static analysis, reading the code, or
a lighter test come first. Following the format the `medical_policies` job
established: plain markdown, no frontmatter, filename-as-identity, one-line
heading + rule + a line or two of why, and short (5 lines, matching the shape
of the four existing policy files). This is a content-only change; the repo has
no build/test tooling, so verification was content review against the
`medical_policies` criteria and the catalog content that actually exists here.

## Changes

TASK-1: Created `policies/playwright-only-if-necessary.md` — prefer static
analysis, reading the code, or a lighter test over browser automation; reach
for Playwright (or a `shot` render) only when those cannot answer the question,
because a browser run is slow and costly. Framed around the universal principle
(prefer the cheapest sufficient verification; reserve heavyweight tooling for
when it is genuinely needed) per the analyst's recommended resolution, with
Playwright named only as the motivating example. The filename keeps the
brief's recognisable identity (`playwright-only-if-necessary.md`) while the
heading states the universal rule ("Browser automation only if necessary").

TASK-2: Audited the new file against the `medical_policies` decision criteria
(universal, short always/never rule, no pure restatement, not situational) and
the catalog content actually present in this repo. Outcome: wording kept
as delivered, no edits needed. Rationale per criterion:

- **Universal / not situational** — the rule is an ordering constraint
  (cheapest sufficient verification first) that applies to every session and
  agent, independent of project, language, or task; it is not a
  "when doing X, …" rule. The heading is the universal principle; Playwright
  and `shot` appear only as concrete examples inside the rule, so it does not
  read as a purely tool-named rule (criterion 1/4).
- **Short** — 5 lines (heading + 4 body lines), same shape as
  `ask-before-adding-dependencies.md` etc. No frontmatter, no id, no README or
  hidden files added to `policies/` (verified: directory contains exactly the
  five `.md` policy files).
- **Adds scope, does not restate** — checked against every always-on and
  on-demand source in the repo:
  - `prompts/meta.md:12-13` ("Prefer concrete verification over assumption:
    build, run, and inspect actual output wherever you can") encourages
    verifying, but does not ration *which* verification path to pick; the
    brief's actual pain — agents reflexively reaching for Playwright instead of
    easy static analysis — is exactly what meta.md does not cover. The policy
    adds the ordering/rationing constraint without contradicting the
    always-on verification guidance.
  - `agents/developer.md:107-129` ("Verifying rendered work", the `shot`
    guidance) — the policy complements it rather than contradicting it: for
    rendered UI, static analysis genuinely cannot answer the question (pixels,
    contrast, overflow), so a `shot` render remains the correct call; the
    policy only rules out browser runs where a cheaper check suffices (e.g.
    non-UI changes). No conflict, so `agents/` was not edited (out of scope).
  - `agents/tester.md:96` (Playwright via `webapp-testing`) and
    `tester.md:50` ("not everything needs a browser") — the tester role already
    carries the instinct as one role-scoped bullet (and the skill itself is an
    on-demand toolkit with a decision tree that already prefers static HTML
    inspection). Promoting it to an always-on, session-wide policy is added
    scope, not a restatement — the same reasoning that kept
    `no-debug-leftovers` in the `medical_policies` job.
  - `skills/webapp-testing` and `skills/frontend-design` — on-demand skills;
    the policy gates *when* to reach for them, it does not restate their
    content.
  - The other four policies (`ask-before-adding-dependencies`,
    `keep-it-committed`, `no-debug-leftovers`, `secrets-hygiene`) — no
    overlap in subject matter.

## Known issues / follow-ups

- The policy is phrased so that the existing "verify rendered UI work with
  `shot`" guidance in `agents/developer.md` stays intact; it only governs
  *when* a browser render is warranted. If a future job wants to soften that
  agent's unconditional "verify UI with shot" wording (e.g. "verify UI with
  shot unless static analysis suffices"), it is a separate change, out of
  scope here.
- The dedupe audit is limited to content present in this checkout; manigot's
  own `internal/session/policies.go` and the "~70 more" skills referenced by
  the `medical_policies` job are not in this repo and were not re-checked
  against the new policy (same caveat as the `medical_policies` job).