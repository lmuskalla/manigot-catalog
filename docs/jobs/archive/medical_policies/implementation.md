# Implementation: policies

id: medical
developer: developer
date: 2026-10-10

<!-- Produced by @developer after implementation. -->

## Summary

Added a new top-level `policies/` directory to the defaults catalog with a
small set of global, always-on policy files, following the brief's format
(plain markdown, one rule per file, filename is the identity, no frontmatter,
short: one-line heading + rule + at most a line or two of why). Four of the
five proposed starter policies shipped; the fifth (`smallest-change`) was
dropped by the TASK-6 audit because it merely restated an existing
system-wide instruction. No code, build, or test tooling exists in this repo,
so verification was content review against the brief's decision criteria.

Final set (sorted as the catalog concatenates them):

- `policies/ask-before-adding-dependencies.md`
- `policies/keep-it-committed.md`
- `policies/no-debug-leftovers.md`
- `policies/secrets-hygiene.md`

## Changes

TASK-1: Created `policies/secrets-hygiene.md` — never commit, log, print or
hardcode secrets/tokens/keys/credentials; read them from env/untracked config.
Universal, always-on, no overlap with an always-on rule in this catalog.

TASK-2: Created `policies/no-debug-leftovers.md` — leave no debug output,
commented-out code, dead code, or stray TODO without a tracked issue behind.

TASK-3: Created `policies/smallest-change.md` — smallest change that satisfies
the task; no refactoring/tidying of adjacent code, no scope expansion.

TASK-4: Created `policies/ask-before-adding-dependencies.md` — do not add a
dependency without explicit approval; a dependency is a lasting project cost.

TASK-5: Created `policies/keep-it-committed.md` — leave no uncommitted work
behind; record changes before finishing.

TASK-6: Audited all five files against the brief's four decision criteria and
the catalog content that actually exists in this repo (`prompts/*.md`,
`agents/*.md`, `skills/`). Outcome: **dropped `policies/smallest-change.md`**;
kept the other four. Rationale per file:

- `secrets-hygiene` — KEEP. Existing coverage is role-specific and on-demand:
  `agents/security.md` lists hardcoded credentials/tokens as a review
  checklist, and `agents/architect.md` mentions secrets management as one
  operational concern. No always-on rule existed; a global policy is a real
  behavioural change. Universal, short, always/never. Passes.
- `no-debug-leftovers` — KEEP. `agents/quality.md:92` carries near-identical
  wording ("no dead code, no commented-out code, no debug output, no TODO
  comments left without a tracking issue"), but that is an on-demand,
  read-only code-quality *review* agent. Promoting it to an always-on rule
  that every session sees is added scope, not a restatement. Passes.
- `smallest-change` — DROP. `prompts/meta.md` is the system-wide layer that
  already reaches every session: "Prefer small, focused, well-understood
  changes over broad sweeping ones. When scope is unclear, ask rather than
  guess — never silently expand a task." and "Don't do things you're not asked
  to do." It is also repeated in `agents/developer.md`, `agents/devops.md` and
  `agents/sysadmin.md`. There is no way to make this meaningfully broader than
  an instruction that is *already* global, so per criterion 3 (restates a core
  instruction with no added scope) it was removed. The file was created under
  TASK-3 and deleted here, as the brief and tasks explicitly anticipated ("the
  final set may be fewer than five").
- `ask-before-adding-dependencies` — KEEP. Only `agents/developer.md:51`
  ("Do not install packages without asking first") covers it, and that is
  developer-role-only; `prompts/meta.md` says nothing about dependencies. The
  policy makes the constraint hold for every session, which is added scope.
  Passes.
- `keep-it-committed` — KEEP. Only `agents/developer.md` has the commit /
  clean-worktree steps, developer-role-only; `prompts/meta.md` does not cover
  committing. As a universal always-on rule it adds scope. Passes.

All four kept files pass the four criteria: universal (every project, language
and task), a short always/never constraint (≤ 3 lines of body), no pure
restatement of an always-on skill or prompt, and not situational. No
frontmatter, no id, no README/hidden files/symlinks in `policies/`.

## Known issues / follow-ups

- The dedupe check in TASK-6 was necessarily limited to the content present in
  this repository. The brief references manigot's `internal/session/policies.go`,
  its `AGENTS.md`, and "~70 more" skills (`go-code-review`,
  `typescript-guidelines`, `security-best-practices`, `test-driven-development`,
  …) that do not exist in this checkout. Those referenced skills may later
  partly overlap with `secrets-hygiene` or `no-debug-leftovers`; if so, this
  set should be re-checked against them.
- `keep-it-committed` is phrased to apply session-wide, but read-only agents
  (`reviewer`, `quality`, `security`, `tldr`) have nothing to commit; the rule
  is a hygiene constraint on final repo state, not an instruction that every
  agent must run `git commit`.
