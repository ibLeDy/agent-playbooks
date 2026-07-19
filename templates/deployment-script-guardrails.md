# Deployment script guardrails

## Goal

Patterns for a deployment script (any language, any target — Compose, bare
systemd, a single VM) that make an accidental wrong-environment deploy hard
to do by mistake, without turning every deploy into a manual ritual. These
are guard patterns to adapt, not a script to copy verbatim.

## Environment markers, checked stricter than the tool itself

If the deployment tool has its own environment-selection mechanism (a Compose
override file, a config profile, an env-specific settings module), don't
trust it alone for production. Require an explicit marker file or variable
that:

- must exist and match an exact expected value (not just "non-empty");
- is a real file, not a symlink (a symlink lets a shared/dev config get
  swapped in silently);
- has restrictive permissions if it can contain or influence secrets;
- causes the script to stop *before* pulling code or touching running
  services if the marker is missing, wrong, or insecurely permissioned —
  fail before mutating anything, not after.

This is deliberately stricter than the underlying tool's own defaults, so a
config mistake can't silently pull in development settings/overrides in
production.

## Deployment markers and idempotent re-runs

Track what was last successfully deployed, separately from what was last
attempted, e.g. as a file under the deploy tool's own state directory (`.git/`
for a git-deployed repo) rather than inferring it from branch/HEAD alone.
Track *sub-stages* separately if they have different failure/retry
characteristics — e.g. "application deployed" vs. "edge cache purged and
verified" are different concerns with different blast radii; a failure in
the second shouldn't force redoing the first.

A re-run of the script should pick up exactly where the last one left off:
skip stages already known-good, retry only what actually failed. This makes
"just run it again" a safe default instead of something that needs a human
to first figure out what state things are in.

## Build only what changed

Before rebuilding an application image/artifact, check whether the inputs
that would affect it (source, dependency manifest, build config) actually
changed since the last successful build. Rebuilding unconditionally on every
deploy wastes time on a resource-constrained host and makes "what changed in
this deploy" harder to reason about after the fact.

## Validate before replacing the running service

Before a candidate build/config replaces what's currently serving traffic:

- run the target's own config-validation entrypoint if it has one (many
  frameworks have a "check config and exit" mode) against both the candidate
  and the production environment file;
- if there's a reverse proxy/gateway config being deployed alongside the
  app, validate its syntax (e.g. `nginx -t` equivalent) before reloading it;
- wait for an actual readiness signal from the new instance before
  considering the deploy complete — not just "the process started."

## Locking

A deploy script that can be invoked concurrently (by a human and a cron job,
or two people) needs an exclusive lock for the duration of a deploy. Prefer
failing fast with a clear "another deploy is in progress" over silently
queuing or, worse, interleaving two deploys against the same host.

## Reconciliation for drifted-but-current state

If code is already at the target revision but the live environment doesn't
match it (a config file was hand-edited on the host, a log-rotation policy
file is missing), the script should detect and reconcile *live* files against
what's tracked in git, not only act on the "code changed" path. Otherwise a
manual hotfix on the host silently becomes permanent because normal deploys
never revisit files that git thinks haven't changed.

## What to document alongside the script

- The exact failure-before-mutation behavior for each precondition check —
  someone debugging a stuck deploy needs to know what "safely stopped" looks
  like versus "partially applied."
- Which user/account deployments should run as, and why (often tied to file
  ownership on the host, not a security preference stated in isolation).
- Override variables/flags and their defaults, if the script supports
  pointing at a non-default location for any of its dependencies (proxy
  config dir, service name, etc.).
- A `--dry-run` or equivalent preview mode, and a `--force` or equivalent
  "reconcile everything regardless of tracked state" mode — both are worth
  having, but the default invocation should do neither implicitly.
