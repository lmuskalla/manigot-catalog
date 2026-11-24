# Implementation: two new agents

id: drama
developer: developer
date: 2026-11-24

<!-- Produced by @developer after implementation. -->

## Summary

Added two read-only conversational catalog agents: `skepticist` (stress-tests
settled decisions with specific questions) and `orchestrator` (coordinates
agents and is the single channel to the human through a room). Both have the
same narrow permission block: no edits, Bash default-deny with only the room
CLI and read-only git allowed. Updated the factual agent counts in the project
context.

## Changes

- **TASK-1 / TASK-2 — `agents/skepticist.md` (new):** frontmatter with
  `name: skepticist`, a one-line trigger-rich description, `tools: Read, Grep,
  Glob, Bash`, `commit: false`, and a permission block (edit deny; Bash default
  deny plus `room`/`mg room`/`mg rooms` and their `*` forms, the read-only git
  commands from `tldr`, then the same destructive-git denies; task/webfetch/
  websearch/question deny). Body defines the role as a professional skeptic (not
  a contrarian), what to challenge, how to ask one grounded question at a time,
  how to work in rooms, what it does not do, and the read-only hard rules.
- **TASK-3 / TASK-4 — `agents/orchestrator.md` (new):** same frontmatter shape
  and permission block, with a coordination-focused description. Body defines
  the room coordinator role, what it does (receive, dispatch, relay, surface
  decisions, report status, stay grounded), how to work in rooms, what it does
  not do (no implementation, reviews, job management, or decisions belonging to
  others/the human), and the read-only hard rules.
- **TASK-5 — consistency and scope audit (no file change):** parsed both
  frontmatters with the YAML parser available in `docs/node_modules`; `name:`
  matches each filename; descriptions are single-line; permission blocks are
  byte-identical; every command mentioned in either prompt is allowed by the
  block (`room`/`--help` via `room *`, `mg rooms`, and the five read-only git
  commands); neither tools list nor prompt implies Write/Edit/commit; no room
  subcommands or protocols are invented. Repo content outside the job
  directory is limited to the two new agent files and the two-number
  `docs/AGENTS.md` edit. A scoped grep (`README.md`, `docs/*.md`, `prompts/`,
  `mcp/`, `policies/`) found no agent index or registration list to update —
  `README.md` is empty, as the analyst expected.
- **TASK-6 — `docs/AGENTS.md`:** changed only the Architecture counts, "17
  agent definitions" → "19" and "11 declare `commit: false`" → "13" (verified:
  19 files, 6 `commit: true`, 13 `commit: false`). The new agent names were not
  added and no other sentence changed.

## Known issues / follow-ups

- **Name collision:** `orchestrator` is also the name of manigot's built-in
  `mg autopilot` agent, and catalog content overrides core content of the same
  name. Installing this catalog globally may shadow that built-in agent. The
  brief explicitly names the new agent "Orchestrator", so the name was kept and
  the built-in copy was not touched. The human should confirm the name before
  installing the catalog globally.
- **Room command surface is not documented in this repo:** the brief calls it
  "mg rooms"; the container uses a `room` binary. The permission block allows
  both, and the prompts deliberately tell the agent to consult `--help` rather
  than name subcommands, flags, or a message protocol. If the real surface
  differs, the allowlist and prompts need a follow-up.
- **No job-management capability:** per the analyst's default, orchestration is
  scoped to rooms; a broad `mg *` allow or the `task` tool stays denied. If the
  human expects the orchestrator to launch jobs directly, that needs a separate
  decision.
