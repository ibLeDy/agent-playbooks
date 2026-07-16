# AGENTS.md size and drift audit

## Goal
Check whether this repo's `AGENTS.md`, combined with the global template, is
healthy: under current provider size caps, not needlessly bloated even where
no cap is at risk, and not drifted from the global template in ways worth
reconciling in either direction.

Trim for two separate reasons, not one: hard provider caps, and general
context pollution. Every byte here is reloaded into every turn for every
tool it's symlinked into, whether or not a hard cap is ever hit — a leaner
file means the load-bearing rules get more relative attention. Don't treat
"under the limit" as "done."

## Steps

1. Measure byte sizes of the global template and this repo's `AGENTS.md`.
2. Verify current size limits for each targeted agent/provider by checking
   its own current docs — don't reuse a previously-recorded figure without
   rechecking, and don't assume one provider's figure applies to another
   (a flat byte cap and a model context-window budget are different kinds
   of constraint). Record the verification date next to whatever figure
   you find.
3. If combined size is at risk of exceeding a cap, work out concatenation
   order before deciding what to trim — some tools load the global file
   first and truncate whatever comes after, so a large global file can
   silently cut off most of the project file even if the project file
   alone looks fine.
3b. Independent of any cap: look for rules that are wordy relative to what
    they actually constrain, redundant with something stated elsewhere, or
    covering a scenario that no longer applies — these are worth trimming
    even with plenty of headroom left.
4. Diff this file against the global template for drift:
   - Refinements this file has that are actually generic → propose
     back-porting them to the global template instead of leaving them
     stuck in one project.
   - Rules the global template already states that this file restates →
     trim the duplicate here.
   - Gaps: rules present in neither that this repo's own history (repeated
     corrections, recent incidents) suggests it actually needs.
5. Report findings before editing anything — this is an audit, not an
   automatic-fix task.
