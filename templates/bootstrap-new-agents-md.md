# Bootstrap a project AGENTS.md

## Context
This repo doesn't have an `AGENTS.md` yet. First determine whether the active
agent is configured to load a global instruction file, what it is called, and
how repository instructions take precedence. If such a global file covers
generic workflow discipline, don't restate it here. This file should carry
only what is actually specific to this repo.

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
- Skip anything an applicable global file already covers (worktree preflight,
  risk communication, git workflow basics, attribution). If no global file
  applies, include the minimum repo-level guidance needed instead.
- If a global file applies, check its current byte size and keep this file's
  size mindful of the combined total against caps the active coding agents
  actually enforce (verify current figures; do not assume).

## Process
1. Discover the active agent's instruction-file names, load order, and
   precedence; read each applicable instruction file in full.
2. Explore the repo: manifests, test/lint config, CI, existing docs.
3. Draft sections only for what's genuinely project-specific.
4. Ask before finalizing anything about deployment/production behavior
   that was inferred rather than confirmed directly.
