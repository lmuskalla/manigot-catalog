---
name: job-brief
description: Use when writing or filling in a job's brief.md for the manigot job workflow. Draft a clear What/Why so the analyst and developer can work from it without guessing.
---

# Writing a job brief

A `brief.md` is the single source of intent for a job. `@analyst` turns it into
tasks, `@developer` implements those tasks, `@reviewer` checks the result
against it — so a vague brief wastes the whole chain.

## Structure

Write only the body of the `docs/jobs/<id>_<slug>/brief.md` template, starting
at `## What`:

- **## What**: what this job changes and how. Concrete, one paragraph plus a bullet list of the notable changes.
- **## Why**: the problem it solves for the user. If you cannot write this in one sentence, you do not understand the job yet.
- **## Out of scope**: what is explicitly NOT in this job, so the developer does not gold-plate.
- **## Notes**: anything the analyst or developer should know before starting.

Never write the frontmatter (`type`, `id`, `branch`, `date`, `author`). It is
generated host-side by `mg job` when the job is scaffolded — job creation is
a host-only operation (the git worktree/branch it requires is outside what an
agent session can do), never something this skill or an agent performs. This
holds whether you're filling in an already-scaffolded `brief.md` (the
frontmatter is already there — leave it alone) or drafting a brief to hand
back for someone to paste into a not-yet-created job.

## Rules

- State intent, not implementation — leave the how to `@developer`.
- If scope is ambiguous, say so in the brief rather than leaving it implicit.
- Keep it short: the analyst reads it first, and a 500-word brief is fine; a 2000-word one is not.
