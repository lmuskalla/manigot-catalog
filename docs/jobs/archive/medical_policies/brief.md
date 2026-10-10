# Brief: policies

type: feature
id: medical
branch: feature/medical_policies
date: 2026-10-10
author: Leander Muskalla

## What

Add a `policies/` directory to the defaults catalog with a small set of
policy files (`policies/<name>.md`). The catalog today has `agents/`,
`skills/`, `mcp/` and `prompts/` — but no `policies/` at all.

## Background: what a policy is

A manigot **policy** is a plain-markdown file, one rule per file, injected
**unconditionally** into every session's project context. Unlike a skill —
which the agent loads only when it judges the one-line description relevant —
a policy is always in effect. Two tiers:

- **Global** — the catalog, installed with `mg defaults install policies/<name>`
  into the user data dir, active in **every project** on the machine.
- **Project** — committed at `<project>/docs/policies/<name>.md`, active only
  there (and, planned, vendorable from the catalog per project).

See manigot's `internal/session/policies.go` and the `Policies` bullet in its
`AGENTS.md` for the mechanism.

## The skills-vs-policies line (read before writing anything)

The catalog already carries deep code-quality and security *knowledge* as
skills: `go-code-review`, `typescript-guidelines`, `python-guidelines`,
`laravel-best-practices`, `security-best-practices`, `test-driven-development`
and ~70 more. That is the right home for that content, and it must not be
duplicated here.

The distinction is **not topic** — skills and policies both touch quality,
security and process. It is **trigger and depth**:

- **Skill** = on-demand depth. A body of knowledge, checklist or workflow,
  loaded when the model judges it relevant. Because it only fires when
  relevant, a catalog skill may be broad and stack-specific.
- **Policy** = always-on constraint. A short rule that must hold whether or
  not the model considers it relevant. Because a catalog policy is global and
  unconditional, it must be **universal**.

Decision rule for any proposed policy:

1. Is it true for every project, every language, every task? If not, it is not
   a catalog policy.
2. Is it a short "always/never" constraint rather than advice or a checklist?
   If it needs more than a few lines, it is a skill.
3. Does it avoid restating a skill, or the core agents' own instructions
   (`agents/developer.md` etc.)? If it just repeats one, don't add it.
4. Is it *situational* ("when writing Go, …")? Then it is a **skill**, or a
   **project** policy — never a global catalog policy.

## Proposed starter set

Small on purpose — quality over quantity. The names/content are a starting
suggestion; the criteria above are the real spec.

- `secrets-hygiene` — never commit, log or print secrets, tokens, keys or
  credentials; read them from env/config, never hardcode.
- `no-debug-leftovers` — leave no debug output, commented-out code, dead code,
  or stray TODO (without a tracked issue) behind.
- `smallest-change` — make the smallest change that satisfies the task; don't
  refactor or tidy adjacent code, and don't expand scope.
- `ask-before-adding-dependencies` — don't add a dependency without explicit
  approval.
- `keep-it-committed` — leave no uncommitted work behind.

## Format

- Path: `policies/<name>.md`. One policy per file; the **filename is the
  identity** — no frontmatter, no id field.
- Plain markdown body: a one-line heading + the rule + at most a line or two
  of why. Keep each file short.
- Files are read verbatim and concatenated (sorted by filename) into the
  session context — no frontmatter, no templating.
- Keep the directory to policy files only; the catalog scan ignores hidden
  files, `README`, and symlinks.

## Non-goals

- No stack- or language-specific rules (those are skills, or per-project
  policies).
- No long documents, checklists or workflows (those are skills).
- No rule that merely repeats an existing skill or the core agent prompts.