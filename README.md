# agent-playbooks

Reusable prompt templates for recurring cross-project, agent-driven tasks —
the kind of work that isn't specific to any one repo, so it doesn't belong in
that repo's own `AGENTS.md`, but is worth not re-deriving from scratch each
time it comes up.

## Using a template

Copy the template's content into a task prompt, fill in or delete the
bracketed/contextual placeholders for the repo at hand, and send it. These are
starting points, not scripts to follow verbatim — adapt anything that doesn't
fit the actual project.

## Design principles

- **Agent-agnostic.** Written for any coding agent (Claude, Codex, Gemini,
  Copilot), not one with a specific plugin/skill installed.
- **Token-conscious.** No rigid mandates to always parallelize, always spawn
  subagents, or always produce extra artifacts — conditional on the task
  actually benefiting from it.
- **Consult before deciding.** Templates default to surfacing decisions and
  asking, not silently picking a direction — matches how this account's
  projects are actually run.

## Templates

- [`improving-agentic-files.md`](templates/improving-agentic-files.md) —
  reconcile a project's `AGENTS.md` against external material or a sibling
  project's file: what applies, what's wrong, what's worth folding in.
- [`bootstrap-new-agents-md.md`](templates/bootstrap-new-agents-md.md) —
  seed a new repo's first `AGENTS.md`, scoped to what's actually
  project-specific rather than restating the global defaults.
- [`agents-md-size-and-drift-audit.md`](templates/agents-md-size-and-drift-audit.md) —
  periodic check that an `AGENTS.md` (combined with the global template) is
  under current provider size caps and hasn't drifted from the global file.
- [`finishing-a-branch.md`](templates/finishing-a-branch.md) — pre-PR and
  post-merge checklist for closing out a feature branch/worktree cleanly.
- [`repo-orientation.md`](templates/repo-orientation.md) — cold-start
  orientation before touching a repo you haven't worked in recently.
- [`dependency-upgrade-request.md`](templates/dependency-upgrade-request.md) —
  structured handling of a "bump this dependency" request.
- [`plan-knowledge-ecosystem.md`](templates/plan-knowledge-ecosystem.md) —
  decide what belongs in git-based agent-facing files vs. a separate
  personal knowledge system, and how the two should relate.
