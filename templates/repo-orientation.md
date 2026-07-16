# Repo orientation (cold start)

## Goal
Get enough context on a repo that hasn't been touched recently before making
any change, without re-deriving everything from scratch every time.

## Steps

1. Read the repo's own `AGENTS.md`/`CLAUDE.md`/`GEMINI.md` in full before
   anything else — assume it overrides generic defaults for a reason.
2. Check recent history: the last ~15 commits, open PRs, and any issues
   flagged for attention.
3. Check for an existing task status file or design doc, if the repo uses
   that convention — don't re-plan work that's already recorded.
4. Confirm how to run tests/lint/build locally before touching code — don't
   assume a command from a similar project transfers here.
5. Note anything that looks like an unresolved blocker (a merge conflict, a
   failing check, a dirty primary worktree) and surface it before
   proceeding, rather than quietly working around it.
