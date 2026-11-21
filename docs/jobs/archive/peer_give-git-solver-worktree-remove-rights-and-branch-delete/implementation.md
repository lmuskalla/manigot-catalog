## Summary

The git-solver agent can now remove and prune worktrees and delete branches. That lets it clean up after a failed `mg done` or a merge conflict. Every other restriction stays as it was. Only `agents/git-solver.md` changed.

## Changes

TASK-1: `agents/git-solver.md` frontmatter. I removed `"git worktree*": deny` and replaced it with explicit denies for `worktree add/move/lock/unlock/repair`. That allows `git worktree list/remove/prune` no matter how glob ordering is evaluated. I also removed the `git branch -d/-D/--delete` denies. `branch --move/--copy` and all other denies are unchanged.

TASK-2: `agents/git-solver.md` body. In "Container limitation", worktree remove/prune and branch delete moved from refused to allowed. The "no exemption" sentence now names this cleanup as the only exemption. The can/cannot lists now include the cleanup operations. Reset, checkout, merge, rebase and worktree repair/move still point to `mg host`. In "How to approach a request", step 5 now includes the cleanup operations. In "Hard rules", the other-branch rule now allows deleting a stray branch or removing an orphaned worktree when the request names it. I added safety rules: before `worktree remove` or `branch -D`, show what will be lost, back up unmerged work with a tag, prefer `-d` over `-D`, and never remove the session's own worktree or branch. The last rule's refused list now matches the permission block.

TASK-3: Consistency check. `name: git-solver` is unchanged and the frontmatter structure is unchanged apart from the swapped entries. I couldn't run a YAML parser because PyYAML and `uv` aren't available in the session. Every command the prompt calls allowed or refused matches the permission block. The only other mention of git-solver's rights is `docs/AGENTS.md`, which already describes wider rights and was left alone.

## Known issues / follow-ups

- `"git reflog*": deny` also blocks *reading* the reflog, but the prompt tells the agent to run `git reflog`. This was already there before this job and is out of scope.
- `git branch -m/-M/-c/-C` (the short forms of `--move`/`--copy`) aren't denied. This was already there before this job.
- The frontmatter `description:` still says "advises safe cleanup". It could say it now performs worktree/branch cleanup, which would help it get picked for these requests.
- `docs/AGENTS.md` says git-solver gets "the full local git toolbox". The agent definition is still narrower than that, so the two should be reconciled.
