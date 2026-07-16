# AGENTS.md size and drift audit

## Goal
Check whether this repo's `AGENTS.md`, combined with the global template, is
healthy: under current provider size caps, and not drifted from the global
template in ways worth reconciling in either direction.

## Steps

1. Measure byte sizes of the global template and this repo's `AGENTS.md`.
2. Verify current size limits for each targeted agent/provider by checking
   its own current docs — don't reuse a previously-recorded figure without
   rechecking; providers change these without notice. Record the
   verification date next to whatever figure you find.
3. If combined size is at risk of exceeding a cap, work out concatenation
   order before deciding what to trim — some tools load the global file
   first and truncate whatever comes after, so a large global file can
   silently cut off most of the project file even if the project file
   alone looks fine.
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
