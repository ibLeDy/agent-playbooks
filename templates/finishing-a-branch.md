# Finishing a branch

## Before opening or marking a PR ready
- Confirm every milestone's tests, lint, and type-checking actually passed —
  not just that the code compiles or the last command exited zero.
- If this repo keeps a per-task status file, confirm it reflects reality:
  nothing marked "pending" that's actually done, no blocker left unresolved
  without being surfaced.
- Re-read the diff end to end once, as a reviewer would, not just from
  memory of writing it.
- Check whether the PR description still matches what actually shipped —
  rewrite it if the implementation moved since it was drafted.

## Cleanup (after merge)
- Fast-forward the primary worktree to updated trunk; don't edit project
  files there.
- Stop any branch-specific stack (Compose project, port-forward, scratch
  session) before removing the worktree.
- Remove only this feature's worktree; delete the merged local branch (and
  remote, if the host didn't already).
- Confirm the primary worktree is clean, on trunk, and synced before
  calling the task done.
