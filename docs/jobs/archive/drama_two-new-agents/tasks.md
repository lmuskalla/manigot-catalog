# Tasks: two new agents

id: drama
analyst: analyst
date: 2026-11-24

<!-- Produced by @analyst from brief.md. -->

## Overview

Add two catalog agent definitions, one file each:

- `agents/skepticist.md` — a read-only agent that stress-tests decisions and
  plans with specific, uncomfortable questions.
- `agents/orchestrator.md` — a read-only agent that coordinates other agents
  and acts as the single channel to the human.

Both need Bash solely for the room feature (`room` in a session; `mg rooms` on
the host), so their permission blocks must allow the room CLI plus read-only
git while denying every other shell command. Both are conversational, not
implementation agents: no `Write`/`Edit`, `commit: false`, no job-file output.

Branch check: the worktree's gitdir HEAD is
`refs/heads/feature/drama_two-new-agents`, matching the brief's `branch:`.

This repository is content-only — there is no build, test suite, or agent
validator. Verification is structural review (frontmatter parses, `name:`
matches the filename, prompt claims agree with the permission block) plus a
grep for stale repo-wide references.

## Open questions (defaults chosen, not blockers)

1. **`orchestrator` collides with a built-in agent.** The installed manigot
   already ships an `orchestrator` agent for `mg autopilot`
   (`~/.config/opencode/agents/orchestrator.md`): read-only, Bash mostly
   denied, and driven by a strict `autopilot-decision` output protocol. The
   catalog is global content and overrides core content of the same name, so
   adding `agents/orchestrator.md` may shadow that agent when the catalog is
   installed. The brief names the new agent "Orchestrator", so the default is
   to follow the brief and create `agents/orchestrator.md` **without touching
   the installed/core copy**, recording the collision in `implementation.md`
   as a follow-up for the human. Do not rename it unilaterally and do not edit
   anything under `~/.config/opencode/` or `.opencode/`. The human should
   confirm the name before installing the catalog globally.
2. **Room command surface is not in this repo.** The container entrypoint
   invokes a `room` binary (`room listen ...`), and the brief calls it
   "mg rooms"; the manigot rooms documentation (`docs/ROOMS.md` in the
   manigot checkout) is not present here. Default: grant Bash for the room CLI
   only (`room`, `room *`, `mg room`, `mg room *`, `mg rooms`, `mg rooms *`)
   and keep the prompts generic — tell the agent to use the room CLI and its
   `--help`, and do **not** invent subcommands, flags, or a message protocol.
3. **Brief's Why/Out of scope are empty.** Treated as "no extra constraints";
   the role paragraphs in `What` are the spec.
4. **`docs/AGENTS.md` counts become stale.** It states "17 agent definitions"
   and "6 declare `commit: true`; 11 declare `commit: false` (read-only)".
   Two read-only agents make that 19 / 6 / 13. TASK-6 updates only those
   numbers; it must not otherwise rewrite project context.
5. **Orchestration is assumed to happen through rooms**, not through `mg`
   job commands or the `task` tool. The brief only asks for shell access for
   rooms, so a broad `mg *` or `task: allow` is out of scope. If the human
   expects the orchestrator to launch jobs directly, the allowlist needs a
   separate decision.

## Task breakdown

### TASK-1 — Author `agents/skepticist.md` frontmatter

- **Description**: Create the frontmatter for the skepticist agent:
  `name: skepticist` (must match the filename), a one-line, trigger-rich
  `description:` (professional skeptic that stress-tests decisions/plans with
  specific uncomfortable questions; use when a plan or implementation looks
  settled), `tools: Read, Grep, Glob, Bash`, `commit: false`, and a
  `permission:` block: `edit: {"*": deny}`; `bash` with `"*": deny` first,
  then allow `room`, `room *`, `mg room`, `mg room *`, `mg rooms`,
  `mg rooms *`, and the read-only git commands used by `agents/tldr.md`
  (`git log`, `git diff`, `git show`, `git status`, `git rev-parse`,
  `git branch --show-current`, each with its `*` form), followed by the same
  destructive-git denies as `agents/tldr.md`; `task`, `webfetch`, `websearch`,
  and `question` all deny.
- **Files**: `agents/skepticist.md` (frontmatter only, new)
- **Depends**: none
- **Risk**: medium — the permission block is the only thing keeping the agent
  read-only; a missing default deny or an over-broad glob (for example
  `"mg *"` or `"*": allow`) would grant unwanted shell access.

### TASK-2 — Write the `agents/skepticist.md` role prompt

- **Description**: Write the body beneath the frontmatter: the role (a
  professional skeptic, not a contrarian), what to challenge (assumptions,
  alternatives, evidence, costs, reversibility, blast radius, opportunity
  cost, scope, user impact), how to ask (one grounded question at a time,
  specific and tied to the actual decision, with a concrete alternative — not
  vague "are you sure?"), how to work in rooms (use the room CLI; messages
  from the human or other agents arrive as prompts; reply plainly; no invented
  commands), and what it does not do (does not implement, edit files, commit,
  nitpick style, duplicate the reviewer, or keep blocking for its own sake).
  End with hard rules: read-only; no file edits/commits; only the room CLI and
  read-only git; no impersonating other agents; no fabricated facts.
- **Files**: `agents/skepticist.md` (body only)
- **Depends**: TASK-1
- **Risk**: low — prose-only, but it must not drift into a second reviewer or
  a generic "devil's advocate" that manufactures doubt, and every command it
  mentions must be allowed by TASK-1's permission block.

### TASK-3 — Author `agents/orchestrator.md` frontmatter

- **Description**: Create the frontmatter for the orchestrator agent:
  `name: orchestrator` (must match the filename), a one-line, trigger-rich
  `description:` (coordinates other agents and is the single channel to the
  human — triage, dispatch, relay, report; use when several agents work in a
  room), `tools: Read, Grep, Glob, Bash`, `commit: false`, and the same
  `permission:` shape as TASK-1 (edit deny; Bash default deny plus only the
  room CLI and read-only git; `task`, `webfetch`, `websearch`, `question`
  deny). Record the built-in-orchestrator name collision and the decision to
  follow the brief in `implementation.md` under known issues.
- **Files**: `agents/orchestrator.md` (frontmatter only, new);
  `implementation.md` (the collision note is added at summary time)
- **Depends**: none
- **Risk**: high — the filename/name matches manigot's built-in autopilot
  orchestrator; if the catalog is installed globally it may shadow that agent,
  so the collision must be documented and left for the human to confirm.

### TASK-4 — Write the `agents/orchestrator.md` role prompt

- **Description**: Write the body beneath the frontmatter: the role (the
  room's coordinator and the human's single point of contact — receives
  requests, breaks them into messages for the right agents, relays answers,
  surfaces decisions, and reports status); what it does (reads the repo and
  job files to stay grounded; talks to the human; routes work through rooms;
  summarizes; asks rather than guesses); how to work in rooms (use the room
  CLI; do not invent commands or protocol); and what it does not do (does not
  implement, edit files, commit, review code, make product/technical decisions
  that belong to another agent, or bypass the human). End with hard rules:
  read-only; no file edits/commits; only the room CLI and read-only git; no
  impersonating other agents; no fabricated status.
- **Files**: `agents/orchestrator.md` (body only)
- **Depends**: TASK-3
- **Risk**: medium — "orchestrating other agents" is broad and can sprawl
  into job management, launching agents, or writing files; the prompt must
  stay scoped to coordination and communication through rooms, and every
  command it mentions must be allowed by TASK-3's permission block.

### TASK-5 — Consistency and scope audit for both agents

- **Description**: Review both new files together: frontmatter parses as YAML
  (use a parser if available, otherwise a careful structural check); `name:`
  matches the filename; the description is on one line and trigger-rich; every
  command the role prompt says is allowed or refused agrees with the
  `permission:` block; no `Write`/`Edit`/`commit` capability is implied;
  no invented room subcommands or protocols; and nothing outside the four
  in-scope files (`agents/skepticist.md`, `agents/orchestrator.md`,
  `docs/AGENTS.md`, `implementation.md`) changed. Grep the repo for agent
  lists or manifests that would need a registration update — none is expected
  (the catalog has no index; `README.md` is empty); record the result.
- **Files**: `agents/skepticist.md`, `agents/orchestrator.md` (read-only check,
  fix only inconsistencies); `implementation.md` (note the audit)
- **Depends**: TASK-1, TASK-2, TASK-3, TASK-4
- **Risk**: low — review-only pass; the main failure is missing a
  prompt/permission mismatch.

### TASK-6 — Update the agent counts in `docs/AGENTS.md`

- **Description**: Change only the factual counts in the `Architecture`
  section: "17 agent definitions" becomes "19 agent definitions", and
  "6 declare `commit: true`; 11 declare `commit: false`" becomes
  "6 declare `commit: true`; 13 declare `commit: false`". Do not add the new
  agent names, do not change any other sentence, and do not edit the generated
  mount mirrors (`/workspace/AGENTS.md`, `.opencode/`, `.claude/`).
- **Files**: `docs/AGENTS.md`
- **Depends**: TASK-1, TASK-2, TASK-3, TASK-4
- **Risk**: low — a two-number factual edit; could be viewed as outside the
  brief, so note it in `implementation.md` if the developer chooses to keep it.

## Notes for @developer

- Work from the source directories only: `agents/` and `docs/`. Never edit
  `/workspace/AGENTS.md`, `.opencode/`, or `.claude/` — they are generated
  mounts.
- There is no agent validator in this repo (the vendored `skills-ref`
  validator is for `skills/` only). Verification is structural review, not a
  test command.
- Follow the frontmatter conventions already used in `agents/`: `name:`,
  `description:`, `tools:`, `commit:`, and a `permission:` block. Do not add
  per-vendor keys.
- Keep the permission blocks narrow. The agents exist to talk through rooms,
  not to run arbitrary shell commands; the read-only git commands are there
  only so they can ground their answers in the repo.
- If the orchestrator name collision or the undocumented room CLI turns out
  to need a product decision, stop and report rather than inventing a name or
  a protocol. Do not rename the agent, and do not edit the installed/core
  orchestrator.
