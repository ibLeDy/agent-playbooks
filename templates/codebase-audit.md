# Codebase audit

## Goal

Produce an independent, evidence-backed audit of a codebase and its
production shape — re-checking claims rather than restating them, and
prioritizing follow-ups — without changing application behavior. This is a
documentation-only deliverable; implementation is separate follow-up work.

## Scope

State up front, in the audit doc itself:

- The exact commit/branch and date being audited.
- What's in scope (source, config, deploy scripts, docs) and explicitly out
  of scope (e.g. live production dashboards, a full paid load test, anything
  requiring production access this pass doesn't have).
- Prior audits or evidence docs this one supersedes or builds on, linked, so
  readers don't have to guess whether an old finding is still current.

## Method

1. Read the actual source for the areas being audited — don't infer behavior
   from documentation or naming alone.
2. Re-verify claims that matter (especially anything already flagged as a
   risk previously) against the current code, with a direct code reference
   or a small local reproduction — not "this was true last time."
3. Cross-check against the test suite: does an existing test actually encode
   the behavior being described? If two code paths are claimed to behave
   differently, look for (or build) a minimal repro that shows it, rather
   than asserting it from reading alone.
4. Classify every finding by severity (high/medium/low) plus optional
   optimization/addition/removal categories — pick categories that fit the
   project, but keep severity separate from "nice to have."

## Finding write-up shape

For each finding worth recording:

- **Status** — new, or re-confirmed from a prior audit (say which one).
- **Evidence** — specific files/lines/tests, not "the search logic seems
  inconsistent."
- **Impact** — who's actually affected and how, not just "this is bad
  practice."
- **Recommended fix** — enough to scope a follow-up PR, explicitly deferred
  to its own change rather than fixed inline in the audit.

A findings table works well for medium/low items where full write-ups would
be excessive; reserve full sections for anything high-severity.

## Suggested lenses (adapt, don't exhaust)

Pick the subset that fits the project instead of running every lens on every
audit:

- Architecture and integration boundaries (does the diagram in the docs
  match what the code actually does?).
- Domain-specific correctness edge cases (whatever the project's core domain
  logic actually is — parsing, protocol handling, state machines).
- Caching/consistency (can two code paths disagree on the same read? is
  staleness bounded?).
- Concurrency/contention (shared state, lock scope, connection pooling).
- Input validation and security (injection, path traversal, authZ/authN
  boundaries, secret handling).
- Deployment/config drift (do docs and actual runtime flags agree?).
- Test coverage and regression risk for what the audit is flagging.
- Licensing/attribution completeness if the project has third-party assets.

## Output structure

A single doc works better than scattered notes:

1. Purpose, scope, method (as above).
2. Architecture snapshot as currently verified (diagram if it helps).
3. Strengths worth naming — an audit that's all findings reads as unbalanced
   and gets discounted.
4. Findings, grouped by severity.
5. Optimizations (priority-ranked, separate from correctness findings).
6. Additions worth considering, split roughly by value/risk — and an
   explicit "defer" list so speculative ideas don't get lost or silently
   promoted later.
7. Removals/cleanup identified along the way.
8. Security posture summary if relevant to the project.
9. Suggested work packages — each sized to its own branch/PR, not one giant
   follow-up.
10. What was and wasn't re-verified for *this* document (be explicit about
    what wasn't run — e.g. "full test suite not re-run, this is a docs-only
    change").

## Non-goals

- Don't fix findings inline as part of the audit PR — that conflates
  evidence-gathering with implementation and makes both harder to review.
- Don't restate a prior audit's findings without re-checking them against
  current code — "still true" is a claim that needs the same evidence bar as
  a new finding.
- Don't pad the strengths section or the findings list to look thorough —
  every entry should carry real evidence.
