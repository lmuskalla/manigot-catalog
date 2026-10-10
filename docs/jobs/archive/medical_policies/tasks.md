# Tasks: policies

id: medical
analyst: analyst
date: 2026-10-10

<!-- Produced by @analyst from brief.md. -->

## Overview

Add a new top-level `policies/` directory to the defaults catalog and author a
small set of global policy files (`policies/<name>.md`), one rule per file,
plain markdown, no frontmatter. The brief's proposed starter set is the basis;
the four decision criteria in the brief ("skills-vs-policies line") are the
real spec and win over the suggested names/content.

Scope boundaries confirmed from the brief:
- Do **not** touch `agents/`, `skills/`, `mcp/`, `prompts/`, or anything under
  `docs/jobs/` other than this job's own files.
- Do **not** add a `README`, hidden files, or symlinks inside `policies/` — the
  brief says keep the directory to policy files only (the catalog scan ignores
  those, but the brief is explicit).
- Every file must be short: a one-line heading + the rule + at most a line or
  two of why. If a rule needs more than that, it belongs in a skill, not here.

Notes for @developer (not blockers):
- The repo contains no build or test tooling; verification is content review
  against the brief's criteria, not `go test`/CI.
- The brief is explicit that names/content are a starting suggestion. Several
  proposed rules (`smallest-change`, `ask-before-adding-dependencies`,
  `keep-it-committed`) overlap with existing core agent instructions
  (`prompts/meta.md`, `agents/developer.md`). Author them as *universal,
  session-wide* constraints (they apply to every agent, not just the
  developer), not as a restatement of the developer prompt. If a rule cannot
  be made meaningfully broader than an existing prompt/skill, flag it in
  `implementation.md` rather than inventing scope.
- The brief's `~70 more` skills and manigot's `internal/session/policies.go` /
  `AGENTS.md` are not present in this repository, so the dedupe check in
  TASK-6 is limited to the files that actually exist here.

## Task breakdown

### TASK-1 — Author `policies/secrets-hygiene.md`

- **Description**: Create the global policy file forbidding committing,
  logging, printing or hardcoding secrets, tokens, keys or credentials
  (read them from env/config instead), written as a short universal
  always/never rule per the brief's format.
- **Files**: `policies/secrets-hygiene.md` (new)
- **Depends**: none
- **Risk**: low — additive markdown file, no code paths or other artifacts
  affected.

### TASK-2 — Author `policies/no-debug-leftovers.md`

- **Description**: Create the global policy file requiring that no debug
  output, commented-out code, dead code, or stray TODO (without a tracked
  issue) be left behind, as a short universal rule.
- **Files**: `policies/no-debug-leftovers.md` (new)
- **Depends**: none
- **Risk**: low — additive markdown file only.

### TASK-3 — Author `policies/smallest-change.md`

- **Description**: Create the global policy file requiring the smallest change
  that satisfies the task (no refactoring or tidying of adjacent code, no scope
  expansion), framed as a session-wide constraint rather than a developer-only
  instruction.
- **Files**: `policies/smallest-change.md` (new)
- **Depends**: none
- **Risk**: low — additive markdown file; the only judgement call is wording to
  avoid merely restating `agents/developer.md` (noted above).

### TASK-4 — Author `policies/ask-before-adding-dependencies.md`

- **Description**: Create the global policy file requiring explicit approval
  before adding a dependency, framed universally for all sessions/agents.
- **Files**: `policies/ask-before-adding-dependencies.md` (new)
- **Depends**: none
- **Risk**: low — additive markdown file; overlaps slightly with the developer
  prompt, so wording must add the session-wide framing.

### TASK-5 — Author `policies/keep-it-committed.md`

- **Description**: Create the global policy file requiring that no uncommitted
  work be left behind, as a short universal rule.
- **Files**: `policies/keep-it-committed.md` (new)
- **Depends**: none
- **Risk**: low — additive markdown file only.

### TASK-6 — Audit the new `policies/` set against the brief's criteria

- **Description**: Compare all files under `policies/` against the brief's four
  decision criteria and the existing catalog content that actually exists here
  (`prompts/*.md`, `agents/*.md`, `skills/`); trim wording, or drop a file
  entirely, if it is not universal, is longer than a short rule, restates a
  skill or a core agent prompt with no added scope, or is situational — and
  record any such decision in `implementation.md`.
- **Files**: `policies/*.md` (may be edited or, if a file fails the criteria,
  removed); `implementation.md` (note the outcome)
- **Depends**: TASK-1, TASK-2, TASK-3, TASK-4, TASK-5
- **Risk**: medium — this is a judgement-based pass that can change or reduce
  the delivered set, so its result should be reviewer-checked against the
  brief's criteria.

## Notes / open questions

- The brief leaves the exact set open ("names/content are a starting
  suggestion"). These tasks author all five proposed policies and rely on
  TASK-6 to enforce the criteria; if a policy fails, the brief's intent is to
  drop it, so the final set may be fewer than five. This is treated as in
  scope, not a scope expansion.
- No human decision was required to produce this breakdown.
