# Verdict: policies

id: medical
reviewer: reviewer
date: 2026-10-10

<!-- Produced by @reviewer and/or @security after implementation. -->

## Review

Branch: `feature/medical_policies` (matches brief). Base: `main` (no
`.manigot/manigot.json` present → fallback). Reviewed via `git diff main...HEAD`.

TASK-1: PASS
notes: `policies/secrets-hygiene.md` present (5 lines: one-line heading +
rule + 2 lines why). Universal, no frontmatter, filename-only identity.
Matches brief wording (never commit/log/print/hardcode secrets; read from
env/config). No always-on rule existed before (only role checklists in
`agents/security.md:70,88,106` and a passing mention in `agents/architect.md`),
so it adds coverage rather than restating an always-on constraint.

TASK-2: PASS
notes: `policies/no-debug-leftovers.md` present, short and universal.
Judgement call verified: the wording closely mirrors `agents/quality.md:92`
("no dead code, no commented-out code, no debug output, no TODO comments left
without a tracking issue"). Criterion 3 warns against restating a core agent's
instructions, but that line is role-scoped to the on-demand `quality` review
agent and never reaches other sessions; promoting it to an always-on rule is
genuinely broader. The brief itself lists `no-debug-leftovers` in the proposed
starter set, so keeping it is within the intended latitude. Acceptable.

TASK-3: PASS
notes: `policies/smallest-change.md` was created (commit 26a6159) then removed
by the TASK-6 audit (commit cd9f132); it is absent from the final diff. This is
the correct outcome and explicitly anticipated by both the brief and tasks.md
("the final set may be fewer than five"). Verified the removal rationale:
`prompts/meta.md:10-11,15` already imposes this session-wide ("never silently
expand a task", "Don't do things you're not asked to do"), so a catalog policy
would be a pure restatement per criterion 3. No deliverable is missing.

TASK-4: PASS
notes: `policies/ask-before-adding-dependencies.md` present, short, framed
session-wide. Only prior coverage is `agents/developer.md:51` ("Do not install
packages without asking first"), which is developer-role-only; the policy adds
all-session scope. Passes the dedupe check.

TASK-5: PASS
notes: `policies/keep-it-committed.md` present, short, universal. Prior
coverage is developer-role-only commit steps in `agents/developer.md`;
`prompts/meta.md` does not cover committing. Adds scope. The
implementation.md caveat (read-only agents have nothing to commit) is honest
and does not undermine the rule.

TASK-6: PASS
notes: Audit performed and fully recorded in `implementation.md`. Dropped
exactly one file (`smallest-change`) with per-file rationale; kept four. Every
dedupe claim spot-checked against the actual repo and confirmed
(`prompts/meta.md`, `agents/quality.md:92`, `agents/security.md`, 
`agents/developer.md:51`). No frontmatter, `README`, hidden files or symlinks
in `policies/` (verified: directory contains exactly the four `.md` files).

Scope: clean. `git diff main...HEAD --name-only` shows only the four
`policies/*.md` files plus this job's own `docs/jobs/medical_policies/`
artifacts (brief/tasks/implementation/verdict/session.log/tokens.json). No
changes to `agents/`, `skills/`, `mcp/`, `prompts/` or anything else. The
`session.log`/`tokens.json` changes are framework bookkeeping, not developer
edits — not a finding.

## Security

No security findings. `secrets-hygiene.md` is itself a security policy and is
phrased correctly (keep secrets out of source, diffs, command output, error
messages). No secret material introduced.

## Overall

APPROVED

All six tasks are satisfied and match the brief's format and decision
criteria. Nothing must change before merge. Non-blocking observations only:

- The TASK-6 dedupe audit was necessarily limited to the content in this
  checkout; the brief's referenced `internal/session/policies.go`, `AGENTS.md`
  and "~70 more" skills are not present here. `implementation.md` records this
  caveat, which is the correct handling.
