---
name: backlog-entry
description: Use when proposing a change to a repository that uses the manigot job workflow but you cannot create a job (no mg access). Write a backlog entry at docs/backlog/<slug>.md whose body a maintainer converts directly into a job brief.
---

# Writing a backlog entry

A backlog entry is how an idea reaches the manigot job workflow when the agent
can't create a job itself. `mg job` scaffolds a job's branch, worktree, and
files host-side, so without `mg` you write a **backlog entry** instead. A
maintainer later converts it: the TUI / web UI "convert to job" action
scaffolds the job and **replaces the new `brief.md`'s content block with your
entry's body**. A good backlog entry is therefore a ready-made brief.

If you do have `mg`, prefer `mg backlog add "<title>"` and pipe the body on
stdin. This skill is for when you don't.

## Where it goes

Write the file directly at `docs/backlog/<slug>.md` in the project's main
checkout — never inside a job worktree. Create `docs/backlog/` if it is
missing.

`<slug>` is the title lowercased, with every run of non-alphanumeric
characters collapsed to a single `-` and leading/trailing `-` trimmed
(`Add Dark Mode` → `add-dark-mode`). If that filename already exists, suffix
`-2`, `-3`, ... until it is free.

## File shape

```
# Backlog: <title>

created: <YYYY-MM-DD>

<body>
```

- `# Backlog: <title>` — the human title. Without it the entry falls back to
  its filename.
- `created: <YYYY-MM-DD>` — required: everything after this line is read as
  the body.
- `<body>` — a complete brief body, starting at `## What`.

No `---` delimiters, no YAML.

## Body

Write the same sections a `brief.md` uses, starting at `## What`:

- **## What** — what should change and how. One paragraph plus a bullet list
  of the notable changes.
- **## Why** — the problem it solves for the user. If you cannot put it in one
  sentence, you do not understand it yet.
- **## Out of scope** — what is explicitly NOT part of this, so the developer
  does not gold-plate.
- **## Notes** — anything the analyst or developer should know before
  starting.

Conversion copies your body in verbatim, so these sections land as the new
brief's sections. Do **not** write `type`/`id`/`branch`/`date`/`author`
frontmatter or a `# Brief:` heading — those are generated when the job is
created.

## Rules

- One idea per entry. Two ideas means two files.
- State intent, not implementation — leave the how to `@developer`.
- If scope is ambiguous, say so in the entry rather than leaving it implicit.
- Keep it short: a 500-word entry is fine, a 2000-word one is not. The analyst
  reads it first.
- The entry is an ordinary docs file: manigot never auto-commits a backlog
  entry, so commit it like any other edit (or leave it for the maintainer).
