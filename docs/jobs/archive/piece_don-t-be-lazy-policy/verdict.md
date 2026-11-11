# Verdict: don't be lazy policy

id: piece
reviewer: reviewer
date: 2026-11-11

## Review

Base: `main` (no `base:` in brief; `.manigot/manigot.json` absent → default).
Diff `main...HEAD`: one new file `policies/dont-take-the-lazy-path.md` plus this
job's own files under `docs/jobs/piece_don-t-be-lazy-policy/`. Nothing outside
scope changed; `agents/`, `skills/`, `mcp/`, `prompts/` untouched.

TASK-1: PASS
notes: `policies/dont-take-the-lazy-path.md` matches the sibling format
(heading, rule, framing line, no frontmatter, trailing newline, 6 body lines).
Covers every point from the tasks.md framing note: choose the better option
even if it costs more code/tokens/effort; never pick or recommend the weaker
option on effort grounds; no silent partial delivery ("finish it or say plainly
what is left"); explicit disclaimer that it does not mean scope expansion or
longer replies. Tool-neutral, kebab-case filename without apostrophes.

TASK-2: PASS
notes: Conflict/duplication check recorded in `implementation.md`. Verified
independently: the last line reconciles with `prompts/meta.md` ("small,
focused changes", "don't do things you're not asked to do"), the
`docs/AGENTS.md` scope rule, and "Keep replies short". No overlap with
`keep-it-committed.md` or other policies. The `playwright-only-if-necessary.md`
cost argument only applies when the lighter check can answer the question, so
it is not a "weaker option" — no contradiction. The `prompts/meta.md` rewording
idea is correctly flagged as an out-of-scope follow-up, not edited.

## Security

none — content-only policy, no secrets or executable content.

## Overall

APPROVED

No blockers.
