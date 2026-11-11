# Tasks: don't be lazy policy

id: piece
analyst: analyst
date: 2026-11-11

<!-- Produced by @analyst from brief.md. -->

## Overview

Add one new global catalog policy under `policies/`. It tells the agent to solve
a task in the best way, not the least-effort way: no picking the easier option
because the better one is a few lines longer, no quietly doing a partial version
of the task, and no letting token or effort cost decide which approach to use.
This is a content-only change: one plain-markdown policy file in the existing
format (a `# Heading`, then the rule, then a line or two of why; no frontmatter;
the filename is the policy's identity, as in `policies/no-debug-leftovers.md` and
`policies/ask-before-adding-dependencies.md`).

Scope boundaries, based on the brief and earlier policy jobs
(`docs/jobs/archive/prior_new-policy-playwright-only-if-necessary/`,
`docs/jobs/archive/medical_policies/`):
- The deliverable is a **global catalog policy** at `policies/<name>.md`, not
  `docs/policies/` (that is the per-project tier).
- Do **not** touch `agents/`, `skills/`, `mcp/`, `prompts/`, or anything under
  `docs/` other than this job's own files.
- Keep the file short, like its siblings: heading + rule + at most a line or two
  of why. Policies are injected into every session, so length has a cost.

## Framing note for @developer (important, not a blocker)

"Be thorough" can easily be misread as "do more than asked". The new policy sits
in the same context as rules that push the other way, and it must not contradict
them:
- `prompts/meta.md`: "Prefer small, focused, well-understood changes…", "never
  silently expand a task", "Don't do things you're not asked to do."
- `docs/AGENTS.md` hard rule: "Keep changes scoped to the agent or skill you were
  asked to touch."
- Policies delivered from outside this repo (visible in the injected context):
  "Keep replies short", "No unnecessary comments", "Ask before adding
  dependencies".

Recommended resolution: make the policy about **how fully and how well the asked
task is done**, not how much is done. Specifically:
- Choose the more correct/robust option even if it costs more code, tokens, or
  effort. Effort is never a reason by itself to recommend the weaker option.
- Finish the whole task. Do not ship a reduced version, a stub, or "the rest is
  left as an exercise" without saying so explicitly.
- When offering options, recommend on merit, not on which one is shorter or
  easier to implement.
- State explicitly that this does **not** mean expanding scope, adding unrequested
  features, or longer replies: thoroughness applies to the work, while scope and
  reply length stay as other rules set them.

Suggested filename: `policies/dont-take-the-lazy-path.md` or
`policies/no-least-effort-shortcuts.md`. Any clear, recognisable kebab-case name
is fine; avoid apostrophes in the filename.

## Task breakdown

### TASK-1 — Author the new policy file in `policies/`

- **Description**: Create a short, universal, tool-neutral policy saying the
  agent must pick the best solution over the least-effort one and must complete
  the full task, regardless of token or effort cost, framed as in the note above.
- **Files**: `policies/<chosen-name>.md` (new)
- **Depends**: none
- **Risk**: low — one new markdown file; the only judgement call is the wording.

### TASK-2 — Check the policy for conflicts and duplication against existing content

- **Description**: Read the new policy alongside `prompts/meta.md`,
  `prompts/interactive.md`, the other `policies/*.md`, and the scope hard rules in
  `docs/AGENTS.md`. Confirm it (a) does not contradict the "small, focused changes
  / don't do what you weren't asked" guidance or the "keep replies short" rule,
  (b) adds a new rule instead of restating an existing one, and (c) stays as short
  as its sibling policies. Fix any issue by rewording the policy file only, and
  record the reasoning in `implementation.md`.
- **Files**: `policies/<chosen-name>.md` (may be edited); `implementation.md`
- **Depends**: TASK-1
- **Risk**: medium — wording that is too strong could push agents into scope creep
  or very long answers in every session, so a reviewer should check the result
  against the meta prompt.

## Notes / open questions

- The brief's `Why`, `Out of scope`, and `Notes` sections are empty, but the What
  is specific enough to proceed. No human decision was needed.
- The brief says "no matter the token or effort cost". I read that as "cost never
  justifies a worse solution", not "spend as much as possible". If the user meant
  the stronger reading, the wording in TASK-1 would need to change.
- If the conflict check in TASK-2 shows that `prompts/meta.md` itself needs
  rewording (for example "Prefer small… changes" being read as "prefer
  low-effort"), that is out of scope here. Flag it in `implementation.md` as a
  possible follow-up job and do not edit `prompts/`.
- This repo has no build or test tooling, and policies are not skills, so
  `validate.sh` does not apply. Verification means reviewing the content.
