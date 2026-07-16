# Improving Agentic Files

## Language
English/American

## Scope
List every repo/file actually in play, and mark which are real vs. aspirational:
- [repo] — AGENTS.md exists / doesn't exist yet (create vs. edit)
- [repo] — ...
(Add other repos here if they should follow the same policy.)

## Goal
Reach conventions we can keep reapplying across the repos above, not a one-off
fix. Prefer additions that work for any coding agent (Claude, Codex, Gemini,
Copilot) reading the file, not ones that assume a specific agent's plugin/skill
is installed.

## Constraints
- Don't mandate rigid workflows (e.g. always fan out N parallel subagents) —
  token cost matters; parallelism should be conditional on the task genuinely
  benefiting from it.
- Keep vocabulary consistent within a file rather than inventing a second
  vocabulary for a concept it already has a term for.
- Editing a repo's instruction file is itself bound by that repo's existing
  rules (branch/worktree discipline, etc.) — the meta-work doesn't get a pass.

## Decision-making
Ask before any important decision or general-direction/design choice. Also
surface small, low-level polish decisions — don't silently pick for me; I want
to know the project well enough to fine-tune it myself.

## Provider file limits
Some coding-agent providers cap instruction-file size (e.g. Codex). For each
targeted provider: confirm the current limit (verify, don't assume from
memory), record it with the date checked and that provider's update cadence so
we know when to recheck, and if content won't fit, surface the trade-off
(split / link out / drop lower-priority guidance) rather than truncating
silently.

## Open questions to resolve as we go
- For any curated file we're revising: does new material apply here, how does
  it differ from current guidance, what's wrong or out of scope, what's worth
  folding in?
- For anything with a "how should this actually be structured" question
  (e.g. one file vs. several, one repo vs. per-project): what would a
  maintainable answer look like, and what would we lose by changing it?

## Feedback loop
Anything worth keeping that surfaces while answering the questions above —
a decision, a fact, a naming choice — gets folded into the relevant curated
file with its reasoning, not left to evaporate at the end of the conversation.

## Done when
State what "done" looks like for this task explicitly before starting (e.g.
"repo X's AGENTS.md updated and merged" vs. "just a discussion/spec").
