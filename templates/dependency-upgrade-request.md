# Dependency/tooling upgrade request

## Before touching the manifest
- Confirm the actual current and target versions — don't assume "latest"
  without checking what that actually resolves to.
- Read the changelog/release notes between current and target for breaking
  changes relevant to how this repo actually uses the dependency.
- Check whether the version is pinned anywhere else too (lockfile, Docker
  base image, CI config) that also needs updating consistently.

## Process
1. Isolate the upgrade in its own branch/worktree — don't bundle it with
   unrelated changes.
2. Apply the bump, update the lockfile if the project uses one, and run the
   full test suite (not just lint/type-check) before considering it done.
3. If anything breaks: decide whether it's a real incompatibility to fix or
   a reason to hold off, and report either way rather than forcing it
   through.
4. Note the upgrade and reasoning in the PR description so future readers
   know why the version changed.
