# Plan the personal knowledge/instruction ecosystem

## Context
Agent-facing operational content can live across multiple git repositories:
a global-instructions repository, a reusable-playbooks repository, and each
project's own instruction files. This works only when the active coding agent
is configured to load those files; a wiki or personal knowledge base must be
explicitly connected before an agent can rely on it.

## Goal
Decide whether this git-based "ecosystem of files" is the right long-term
model, or whether a complementary personal knowledge system (e.g. an
Obsidian vault) should hold some of this instead — and if so, exactly which
content goes where.

## Questions to resolve
- What's actually agent-facing (must be a plain file in a repo an agent
  reads automatically) vs. personal/reflective (decisions, ideas,
  cross-project learnings only the user needs to browse/search/link)?
- If a second-brain/PKM layer is added, is it a replacement for any part of
  the git ecosystem, or strictly additive/complementary?
- How much duplication risk exists if the same fact needs to live in both
  places, and what's the rule for which one is authoritative?
- What's the actual maintenance cost of each option — keeping several
  project AGENTS.md files in sync with one global template, vs. also
  keeping a vault in sync with the git repos?
- Do the existing repositories imply a natural boundary (global instructions
  = shared workflow, playbooks = reusable task templates, project instructions
  = project specifics) that a PKM layer would sit alongside rather than
  replace?

## Constraints
- Don't move anything an agent needs to read automatically out of a git
  repo into a system agents can't read without being told to fetch it.
- Prefer decisions that reduce total maintenance burden, not just relocate
  it.
- Land on a recommendation with trade-offs — don't unilaterally pick a
  specific tool/format.

## Process
1. Inventory what currently exists and where.
2. Classify each piece of content by the questions above.
3. Propose 2-3 concrete models (e.g. "git-only," "git + Obsidian for
   reflective content only," "git + Obsidian mirrors as read-only links")
   with trade-offs and a recommendation.
4. Only after agreement, write the decision down somewhere durable.
