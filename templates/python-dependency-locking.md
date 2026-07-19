# Python dependency locking

## Goal

Move a Python project from "pin direct dependencies, resolve transitives at
build time" to a fully locked, reproducible install — so a new compatible
transitive release can't silently change what ships, and every install (dev,
CI, production image) resolves to the same graph.

Applies to applications. If the project is a published library, checking in a
full resolved environment is the wrong move — stop and confirm which shape
this actually is before proceeding.

## Before starting

- Confirm the current install path: what installs the project today (`pip
  install .`, `pip install -e .[dev]`, a requirements file, etc.) and where
  transitive resolution currently happens (locally, in Docker, in CI — maybe
  all three, independently).
- Confirm the actual dependency manifest is the source of truth
  (`pyproject.toml` direct pins) — don't lock against something already stale.
- Pick a locking tool deliberately: `uv` (checked-in `uv.lock`) is a common
  default because it reads existing `pyproject.toml`, supports optional/dev
  dependency groups, produces a cross-platform lock, and can fail closed when
  metadata and lock disagree. A PEP 751 `pylock.toml` or hash-pinned
  `requirements.txt` are acceptable alternatives if they give equivalent
  guarantees with less workflow overhead for this repo. Don't default to a
  tool just because it's popular elsewhere in the org — check what the
  project's build/CI already assumes.

## Phase 1: resolution spike (no version bumps)

1. Pin the locking tool's own version wherever it runs (Docker, documented
   local commands, CI).
2. Generate a lock from the *unchanged* manifest — this phase does not update
   any package version.
3. Diff the locked graph against what's actually installed in the current
   production image/environment. A first-generated lock choosing different
   transitives than what's deployed today is expected, not a bug — but it
   must be reviewed as a real dependency change, not waved through as
   "just tooling."
4. Check platform markers if the project runs on more than one OS/arch
   (e.g. macOS dev machine vs. Linux container) — packages with compiled
   wheels are the usual failure point.
5. Confirm the dev/test dependency group locks to the same environment tests
   currently run against.

## Phase 2: build integration

Choose one, and document why:

- **Tool-managed image**: copy the manifest + lockfile (+ minimal package
  stub) into an early Docker layer, install frozen/locked, then copy
  application source after — preserves layer caching. Install dev
  dependencies only behind an explicit build arg/target, never by default in
  the production image.
- **Exported hash-pinned requirements**: generate a hash-pinned runtime file
  and a separate dev file from the authoritative lock, install with
  `--require-hashes`, and add a check that the exported files are still
  current relative to the lock. Only worth it if the tool-managed path is
  materially worse for this project's Docker/CI setup — don't hand-maintain
  exported files alongside a lock as a permanent arrangement.

Either way: the production image must not contain dev-only tooling (test
runner, linter, type checker, etc.) after this change.

## Phase 3: update workflow

Document, in the project's own docs (not here), commands for:

- verifying the lock matches the manifest (should be a CI/pre-commit gate);
- updating one named package vs. updating everything intentionally;
- reviewing a dependency-tree diff before merging an update;
- rebuilding both production and dev images from the new lock;
- rolling back by reverting the lock and rebuilding.

If Dependabot (or an equivalent) is in use, confirm it updates the lock in
the same PR as the manifest bump — a dependency PR must not be mergeable with
a stale lock. Disable the automation for this ecosystem until it can do that
reliably, rather than merging PRs that reintroduce drift.

## Validation before adoption

1. Build the production image twice from a clean context; compare installed
   package/version lists — they must match.
2. Build the dev image and run the full test suite (not just lint/type-check)
   at whatever coverage bar the project already holds.
3. Confirm the production image excludes dev-only packages.
4. Confirm Docker layer caching still skips dependency installation when only
   application source changed.
5. Confirm a manifest edit made without updating the lock fails the frozen
   install/check — this is the actual guarantee being adopted.
6. If the resolved network/TLS stack changed, run whatever live smoke test
   the project has for that path.

## Adoption criteria

Adopt when: both environments (runtime, dev/test) are fully resolved; the
production image isn't materially larger or slower; there's one clear
single-package update workflow; automated dependency PRs keep manifest and
lock in sync; and rollback is "revert the lock, rebuild."

Reject or revise if contributors end up maintaining more than one
authoritative dependency file, or the lock can't represent every environment
the project actually needs.

## Rollback

Restore the previous install commands and drop the lock-specific CI/pre-commit
checks. The manifest stays the authoritative direct-dependency list either
way.
