# Repo orientation (cold start)

## Goal
Get enough context on a repo that hasn't been touched recently before making
any change, without re-deriving everything from scratch every time.

## Steps

1. Discover which instruction-file names and precedence rules the active agent
   supports. Read the applicable repository instructions in full before
   acting; do not assume similarly named files all apply or have equal weight.
2. Check recent history: the last ~15 commits, open PRs, and any issues
   flagged for attention.
3. Check for an existing task status file or design doc, if the repo uses
   that convention — don't re-plan work that's already recorded.
4. Confirm how to run tests/lint/build locally before touching code — don't
   assume a command from a similar project transfers here.
5. Note anything that looks like an unresolved blocker (a merge conflict, a
   failing check, a dirty primary worktree) and surface it before
   proceeding, rather than quietly working around it.
