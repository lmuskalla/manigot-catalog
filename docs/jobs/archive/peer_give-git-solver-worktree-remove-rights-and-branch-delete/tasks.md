# Tasks: give git-solver worktree remove rights and branch delete

id: peer
analyst: analyst
date: 2026-11-21

## Context

The only file in scope is `agents/git-solver.md`. Two parts of it block the
cleanup the brief asks for:

1. The frontmatter `permission:` block (passed through to OpenCode) denies
   `git worktree*`, `git branch -d*`, `git branch -D*`, and `git branch --delete*`.
2. The role prompt ("Container limitation", step 4 of "How to approach a
   request", and the last two "Hard rules") tells the agent that worktree
   management and branch deletion are refused, so it should send the user to
   `mg host`.

manigot's own context (`docs/AGENTS.md`, around line 106) already says the
session git shim gives @git-solver "the full local git toolbox (only network
git — push/fetch/pull/remote — stays refused)". That means the platform shim
should not block these commands anymore. The catalog definition is the stale
part. I couldn't check this against the actual shim from this repo.

Branch check: `.git` points at the worktree
`peer_give-git-solver-worktree-remove-rights-and-branch-delete`, which matches
the brief's `branch:`. I couldn't run `git branch --show-current` because Bash
isn't available to the analyst here.

## Open questions (scope is unclear, so not guessed)

- **How wide the worktree allowance is.** The brief says "worktree remove".
  Cleaning up after a failed `mg done` usually also needs `git worktree prune`
  for stale registrations, and sometimes `git worktree list` (read-only, but
  currently denied by `git worktree*`). Default for the developer: allow
  `git worktree list*`, `git worktree remove*`, and `git worktree prune*`, and
  keep `add`/`move`/`lock`/`unlock`/`repair` denied. Widen or narrow this if the
  human disagrees.
- **Whether to adopt the "full local toolbox" from the manigot docs.** That
  would also lift reset/checkout/clean/stash/etc. The brief doesn't ask for
  that, so it is **out of scope**. Only worktree removal and branch deletion
  change.
- `git branch --move*` / `--copy*` stay denied, because the brief only asks for
  delete.

## Task breakdown

TASK-1: Relax the `permission:` bash deny-list in `agents/git-solver.md` so
worktree list/remove/prune and `git branch -d/-D/--delete` are allowed, and keep
every other deny entry as it is.
  files: agents/git-solver.md (frontmatter only)
  depends: none
  risk: medium — glob ordering matters (`"git worktree*": deny` has to be
  replaced by specific allow/deny entries, not left next to them), and a
  too-broad pattern would quietly allow `worktree add/move`.

TASK-2: Update the role prompt in `agents/git-solver.md` so it matches the new
rights. Specifically:
  - "Container limitation": remove worktree management and branch deletion from
    the refused list and the "You cannot" list, and keep the rest (reset,
    checkout, push, ...) pointing at `mg host`.
  - "How to approach a request": let step 4/5 include removing a stuck
    worktree or deleting a stray branch.
  - "Hard rules": rewrite the last bullet's refused list to match. Change
    "Never touch any other branch than the one you were invoked on" so it
    explicitly allows deleting a stray/leftover branch or removing an orphaned
    worktree when the cleanup request names it.
  - Add a safety rule: before any `worktree remove` or `branch -D`, show what
    will be lost (unmerged commits, uncommitted changes), leave a backup ref
    (for example a tag) for unmerged work, prefer `branch -d` over `-D`, and
    never remove the worktree or branch the session is currently running in.
  files: agents/git-solver.md (body only)
  depends: TASK-1
  risk: medium — the prompt has to stay consistent with the permission block
  and must not suggest the agent can now do reset/checkout/merge, which are
  still refused.

TASK-3: Check the edited file for consistency. The frontmatter should still
parse as valid YAML, `name:` should still match, and every command the prompt
says is allowed or refused should agree with the `permission:` block. Confirm
nothing outside `agents/git-solver.md` needs to change (grep found no other
references to git-solver's refused commands besides `docs/AGENTS.md`, which
already describes the wider rights and must not be edited).
  files: agents/git-solver.md (read-only check)
  depends: TASK-1, TASK-2
  risk: low — review-only step; no skill validator applies to agent files.
