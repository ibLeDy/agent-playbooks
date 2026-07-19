# Publishing a private repository as open source

## Goal

Decide, deliberately, whether a private repository should become public —
and if so, get through the checklist once rather than fixing gaps after the
fact. Treat the visibility change itself as effectively irreversible: once
public, commit history and everything in it can be copied even if the
repository is made private again later.

## Frame the decision first

Before any checklist work, get explicit answers to:

- Is there a proprietary dataset, algorithm, or customer-specific logic in
  here that publishing would actually give away? Not "does this feel private"
  — a concrete asset.
- Does publishing make the running system easier to abuse (attack surface,
  rate-limit assumptions, endpoint behavior become inspectable)? This is a
  real cost even for otherwise-fine-to-publish code.
- What posture is this actually becoming: a maintained open-source project
  (implies support expectations), a source-visible reference (code readable,
  contributions not really invited), or a portfolio piece? Pick one — it
  changes what the P1 checklist below needs.

State a recommendation and the reasoning before touching the checklist. If
the answer is "not yet," stop there — don't run the checklist just because it
exists.

## P0 — before changing visibility (blocking)

1. **License posture.** Choose one explicitly: permissive (MIT/Apache-2.0),
   network-reciprocal (AGPL, only if that tradeoff is actually wanted), or
   source-visible/no-redistribution notice. An empty or missing `LICENSE`
   file left in a "public" repo is source-visible, not open source — treat
   that as a defect to fix, not a neutral default.
2. **Full-history secret scan.** Run a dedicated scanner (not a manual
   read-through) over the entire git history, not just the current tree.
   Resolve every finding — rotate the credential if a real secret was ever
   committed, even in a squashed/old commit — before proceeding.
3. **Asset/redistribution review.** Every tracked third-party asset needs a
   license check: vendored frontend libraries, logos/trademarks belonging to
   others, generated images, base images. Update the notices file so it's
   actually complete, not just "the one dependency someone remembered."
4. **Operational-detail review.** Decide whether infrastructure runbooks
   (deploy topology, internal hostnames, specific hardening steps) belong in
   the public repo or should split into a private ops doc. Default to
   splitting if the doc names real infrastructure rather than generic
   patterns.
5. **Public-facing disclaimers**, if the product could be over-trusted:
   state what it does and does not guarantee (accuracy, completeness, SLA)
   before strangers start relying on it.
6. **Contact surface.** A security-reporting path that does not require
   filing a public issue (private email, `SECURITY.md`, a security.txt) —
   this must exist before publishing, not get added afterward once someone's
   already found something.

Do not proceed past this list with any item still open.

## P1 — immediately after publishing

1. Turn on every repo protection the plan allows: required PRs on the
   default branch, force-push disabled, branch deletion disabled, secret
   scanning + push protection, dependency alerts.
2. Decide on CI visibility — public contributions are much easier to review
   with a visible check run than "trust me, it passed locally."
3. Add issue templates: bug report, and (if relevant) a template that
   redirects security reports away from the public tracker.
4. Add a minimal `CONTRIBUTING.md` only if outside contributions are actually
   wanted — don't imply an open contribution process the project doesn't
   intend to run.
5. Decide on tags/releases deliberately; don't tag just to look active.

## Sequence

1. Do all P0 work on a branch/PR against the still-private repo; merge it.
2. Re-verify the default branch is clean and current, and that any published
   docs still match the intended posture.
3. Run the secret scan one final time, immediately before flipping
   visibility — not hours/days earlier.
4. Change visibility.
5. Apply P1 protections immediately — don't leave a public unprotected
   window.
6. View the repo logged out to confirm what a stranger actually sees.
7. Confirm the security-reporting path works end to end.

## Explicit non-goals

- Don't publish internal ops notes just to "prove the project is real."
- Don't cut a release before the version/changelog/deployment policy is
  actually meaningful.
- Don't oversell what the project does in public docs to make it look more
  complete than it is.
- Publishing is not a substitute for the monitoring/backup/abuse-control work
  the project needs regardless of visibility.
