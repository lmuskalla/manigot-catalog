# Implementation: don't be lazy policy

id: piece
developer: developer
date: 2026-11-11

## Summary

Added a global catalog policy, `policies/dont-take-the-lazy-path.md`. It tells
the agent to pick the best solution over the least-effort one, to complete the
whole task (or say plainly what is left), and never to let code, token, or
effort cost alone justify a weaker option. It also says this does not mean
expanding scope or writing longer replies.

## Changes

TASK-1: New `policies/dont-take-the-lazy-path.md` in the sibling format: heading,
rule, one line of framing, no frontmatter. Seven lines, close to the 5–7 lines
of the other policies.

TASK-2: Checked for conflicts and duplication. No rewording was needed.
- `prompts/meta.md` ("small, focused changes", "never silently expand a task",
  "Don't do things you're not asked to do") and the `docs/AGENTS.md` scope rule:
  the policy covers how fully and how well the asked task is done, not how much
  is done. Its last line says it does not justify scope expansion, so the two
  fit together.
- "Keep replies short": the policy says it does not call for longer replies.
- `policies/playwright-only-if-necessary.md` prefers lighter verification. That
  holds only when the lighter means "can answer the question", so it is not
  the "weaker option" the new policy rules out. No conflict.
- Duplication: no existing policy or prompt covers choosing on merit over effort
  or not shipping partial work silently. `keep-it-committed.md` is about
  uncommitted state, not incomplete tasks.

## Known issues / follow-ups

- Possible follow-up, out of scope here: "Prefer small, focused … changes" in
  `prompts/meta.md` could be read as "prefer low-effort". It could be reworded
  to say "small in scope", but `prompts/` was not touched.
