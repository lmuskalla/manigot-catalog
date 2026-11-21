# Verdict: give git-solver worktree remove rights and branch delete

id: peer
reviewer: reviewer
date: 2026-11-21

## Review

Reviewed `git diff main...HEAD`. The only change outside the job directory is `agents/git-solver.md`. Everything else in the diff is job-workspace files (brief, tasks, implementation, verdict, session.log, tokens.json).

TASK-1: PASS
notes: In the `permission:` block, `"git worktree*": deny` was replaced with explicit denies for `worktree add/move/lock/unlock/repair`. With `"*": allow`, that leaves `git worktree list/remove/prune` allowed. It doesn't depend on glob ordering, and nothing broader slipped through. The `git branch -d/-D/--delete` denies were removed. Every other deny entry is unchanged. `"git prune*"` doesn't match `git worktree prune`, so prune works as intended. `name:` and the frontmatter structure are intact.

TASK-2: PASS
notes: "Container limitation", the can/cannot lists, step 5 of the approach, and the "Hard rules" all now match the permission block. Reset, checkout, switch, merge, rebase, and worktree repair/move still point to `mg host`. The other-branch rule now has a cleanup exception scoped to what the request names. The safety rules are all there: show what will be lost, leave a backup tag for unmerged work, prefer `-d` over `-D`, and never remove the session's own worktree or branch. The suggested backup command, `git tag backup/<name> <sha>`, is allowed by the permission block, which only denies `git tag -d*`.

TASK-3: PASS
notes: The prompt's allowed and refused commands agree with the permission block. Nothing outside `agents/git-solver.md` needed changing, and `docs/AGENTS.md` was left alone as instructed. The follow-ups listed in `implementation.md` were already there before this job (reflog read deny, the short `-m/-M/-c/-C` forms) or are outside the brief (wording in `description:`). None of them blocks this job.

## Security

none

## Overall

APPROVED

No changes required.
