# Infrastructure audit and change

## Goal

Understand the named environment, identify its risks and dependencies, and
make only approved, reversible changes. This is platform-agnostic: do not
assume a cloud provider, hypervisor, operating system, container runtime, or
deployment tool.

## Scope and guardrails

- Name the target environment, owner, access boundary, and requested outcome.
- Start read-only. Record confirmed facts separately from assumptions and
  questions.
- Do not expose, print, commit, or copy credentials, private keys, tokens,
  cookies, private hostnames, IP addresses, or internal topology. Redact them
  in reports and use placeholders in examples.
- Before any change that affects data, access, availability, security posture,
  spend, or external exposure, provide the plan, blast radius, validation,
  rollback, and whether explicit approval is required.
- Prefer the narrowest inspection and change that answers the question. Check
  for shared locks, production contention, rate limits, and cost before
  running an infrastructure command.

## Discovery

1. Identify the platform and its exact version, deployment mechanism, and
   authoritative configuration sources.
2. Inventory workloads, versions, ownership, data stores, mounts/volumes,
   identities, network exposure, DNS/TLS, backups, monitoring, and scheduled
   jobs.
3. Map dependencies and trust boundaries: client to edge, edge to service,
   service to data store, and service to third parties.
4. Check recent changes, alerts, health, restore evidence, and open incidents.
5. State what remains unknown and the minimum safe way to learn it.

## Plan before changing

For each proposed change, document:

- Current and desired state, with the evidence for each.
- Reason, dependencies, risk, blast radius, and maintenance window needs.
- Exact change, pre-change backup or snapshot, validation, and rollback.
- Whether it changes user-visible behavior, data retention, network exposure,
  permissions, credentials, cost, or availability.
- The approval required before execution.

## Execution and validation

1. Apply one independently reversible change at a time.
2. Keep data and configuration backups until restore validation succeeds.
3. Validate the intended path and relevant failure paths; do not infer success
   from a command exit code alone.
4. Re-check access control, logs, monitoring, and backup health after each
   material change.
5. Record the result, deviations, evidence, and any remaining follow-up.

## Deliverable

Produce a concise current-state summary, dependency/trust-boundary diagram
where useful, risk register, ordered change plan, rollback procedures, and
open questions. Store environment-specific plans with the repository that
owns the environment; keep this template generic.
