# Bootstrap a project AGENTS.md

## Context
This repo doesn't have an `AGENTS.md` yet. A global default file already
exists (symlinked into `~/.codex/AGENTS.md`, `~/.claude/CLAUDE.md`,
`~/.grok/AGENTS.md`) and covers generic workflow discipline — don't restate
it here. This file should only carry what's actually specific to this repo.

## Discover before writing anything
- Stack and entry points: language, framework, package manager, how it's run.
- How to run tests, lint, type-checking, and a build locally — verify by
  actually running them, don't infer from a similar project.
- How the project is deployed, if at all, and what "production" means here.
- Existing docs conventions — is there already a pattern to match rather
  than inventing a new one?
- Existing CI config, so this file reflects it accurately instead of
  duplicating or contradicting it.

## What belongs in this file
- Only what's genuinely project-specific: stack, exact commands, file
  layout, this project's own docs/testing/deployment conventions.
- Skip anything the global file already covers (worktree preflight,
  risk-communication labels, git workflow basics, attribution) — it
  applies automatically; restating it just adds bytes and drift risk.
- Check the global file's current byte size first, and keep this file's
  size mindful of the combined total against whatever cap the coding
  agents in use actually enforce (verify current figures, don't assume).

## Process
1. Read the global `AGENTS.md` in full so nothing gets duplicated.
2. Explore the repo: manifests, test/lint config, CI, existing docs.
3. Draft sections only for what's genuinely project-specific.
4. Ask before finalizing anything about deployment/production behavior
   that was inferred rather than confirmed directly.
