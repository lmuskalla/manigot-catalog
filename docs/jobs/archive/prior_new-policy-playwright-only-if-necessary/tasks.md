# Tasks: new policy: playwright only if necessary

id: prior
analyst: analyst
date: 2026-10-11

<!-- Produced by @analyst from brief.md. -->

## Overview

Add one new global catalog policy under `policies/` capturing the brief's rule:
browser automation (Playwright, and the `shot` render path that drives it) is
expensive, so it should be reached for only when it is genuinely necessary or
helpful — static analysis and cheaper verification come first. This is a
content-only change: a single plain-markdown policy file, no frontmatter,
following the format the `medical_policies` job established (one-line heading +
the rule + at most a line or two of why; the filename is the identity).

Scope boundaries confirmed from the brief:
- The deliverable is a **global catalog policy** at `policies/<name>.md` — the
  brief says "create a new policy", and this repo's source-of-truth policy
  directory is the top-level `policies/` (not `docs/policies/`, which is the
  per-project tier described in `docs/AGENTS.md`).
- Do **not** touch `agents/`, `skills/`, `mcp/`, `prompts/`, or anything under
  `docs/` other than this job's own files.
- Do **not** add a `README`, hidden files, or symlinks inside `policies/` — keep
  the directory to policy files only.
- Keep the file short: heading + rule + at most a line or two of why.

## Framing note for @developer (important, not a blocker)

The `medical_policies` brief's decision criteria for a global catalog policy
are the real format spec here: a policy must be **universal** (true for every
project/language/task) and must **not be situational or tool-specific** — if a
rule is "when doing X, …", it belongs in a **skill** or a **project** policy,
never a global catalog one. A rule named purely after one tool (`playwright`)
reads as situational and would fail criterion 1/4.

Recommended resolution: frame the policy around the **universal principle** —
prefer the cheapest sufficient verification; reserve slow/heavyweight tooling
(browser automation such as Playwright, or `shot` renders) for when static
analysis, reading the code, or lighter tests genuinely cannot answer the
question — and name Playwright as the concrete motivating example rather than
the whole subject. Suggested filename: `policies/playwright-only-if-necessary.md`
(the filename is the policy's identity and should stay recognisable).

Known tension to record (do **not** silently expand scope): `agents/developer.md`
lines ~107-129 instruct the developer to verify UI changes with `shot`, and
`agents/tester.md` line ~96 points at the `webapp-testing` skill (Playwright).
The policy must **complement** those role instructions, not contradict them;
word it so it governs *when* to reach for the browser, leaving the existing
"verify rendered UI work" guidance intact. If a genuine contradiction cannot be
avoided without editing an agent file, stop and flag it in `implementation.md`
rather than editing `agents/` (that is out of scope for this brief).

## Task breakdown

### TASK-1 — Author `policies/playwright-only-if-necessary.md`

- **Description**: Create the global policy file carrying the brief's rule —
  browser automation / Playwright is a costly verification path, so use it only
  when it is strictly necessary or genuinely helpful; prefer static analysis and
  cheaper checks first. Write it as a short, universal, session-wide always/
  never constraint (heading + rule + a line of why), tool-neutral in spirit with
  Playwright named only as the motivating example.
- **Files**: `policies/playwright-only-if-necessary.md` (new)
- **Depends**: none
- **Risk**: low — additive markdown file only; the only judgement call is the
  universal framing (see note above), not any code path.

### TASK-2 — Audit the new policy against the existing catalog

- **Description**: Check the new file against the `medical_policies` criteria
  and the catalog content that actually exists here: shorten wording if it
  exceeds a short rule, and confirm it (a) is universal, (b) adds scope rather
  than restating the `webapp-testing`/`frontend-design` skills or
  `prompts/meta.md`, and (c) does not contradict `agents/developer.md` (the
  `shot` render guidance) or `agents/tester.md` (Playwright via
  `webapp-testing`). Adjust the phrasing to resolve any conflict within the
  policy file; record the dedupe/consistency reasoning and any decision that
  cannot be made in-file in `implementation.md`.
- **Files**: `policies/playwright-only-if-necessary.md` (may be edited);
  `implementation.md` (record the outcome)
- **Depends**: TASK-1
- **Risk**: medium — a judgement-based pass that can change or trim the
  delivered wording, so its result should be reviewer-checked against the
  brief's criteria and the "add scope, don't restate" rule.

## Notes / open questions

- The brief's `Why`, `Out of scope`, and `Notes` sections are empty; the What is
  specific enough to proceed. No human decision was required. The main open
  design point — universal-principle framing vs. a Playwright-named rule — is
  resolved in favour of the universal framing above, because a situational rule
  does not qualify as a global catalog policy; if the reviewer disagrees, the
  alternative is a `docs/policies/` project rule or a skill, which is a larger
  decision than this brief describes.
- This repo has no build/test tooling; verification is content review against
  the policy format and criteria, not `go test`/CI.
- If a follow-up is ever wanted (e.g. softening `agents/developer.md`'s
  unconditional "verify UI with shot" wording), it is out of scope here and
  should be tracked as its own job.
