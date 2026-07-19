# Small-VPS operations baseline

## Goal

Produce or refresh a durable ops runbook for a service running on a small,
single-host VPS (not a managed cluster/PaaS) — the recurring baseline table,
resource guardrails, and log/disk hygiene that every small deployment needs
but that's easy to skip until the disk fills up. Platform-agnostic: works for
Docker Compose, bare systemd services, or a mix.

Keep the produced runbook in the repository that owns the environment, not
here — this template is the shape, not the content.

## Baseline table

Record durable facts, not point-in-time usage (usage is observed with
commands, not hand-copied into the doc and left to rot):

| Field | Example |
| --- | --- |
| Operating system | Ubuntu 24.04 LTS |
| CPU | 1 vCPU |
| Memory | ~850 MiB |
| Swap | none / size |
| Root filesystem | ~10 GiB |
| Container/process runtime | Docker + Compose plugin / bare systemd |
| Public path | edge → reverse proxy → app process |

State explicitly what the host is *not* sized for (extra workers, a second
service, a sidecar) and why — a memory/CPU-constrained host usually has one
process-count assumption baked into the architecture (e.g. one worker means
one in-memory cache; adding workers without redesigning that breaks
correctness, not just performance). Say what would have to change before
scaling out.

Update this table only when the host is actually resized, the OS changes, or
ownership moves — not on every review.

## Resource guardrails

A warning/urgent threshold table, tuned to the actual baseline above, for the
handful of signals that matter on a small host:

| Signal | Warning | Urgent |
| --- | --- | --- |
| Root filesystem | e.g. 75% used | e.g. 85% used or < 1 GiB free |
| Available memory | sustained low-water mark | OOM-killer event |
| Process/container restarts | any unexplained restart | repeated / crash loop |
| App-specific health signal | degraded | not-ready / error rate |

If there's an edge cache (CDN, reverse-proxy cache) in front of the origin,
say so explicitly: host-level request counts are then origin traffic only,
not total site traffic, and the two must not be confused when reading
dashboards.

## Log and disk hygiene

Two independent things commonly get missed on a small host:

1. **System journal** (if systemd): bound persistent and runtime journal size
   explicitly rather than trusting the default. Document the drop-in config,
   how to inspect the *merged* effective config (not just the main file, in
   case other drop-ins exist), and how to vacuum after a limit change.
2. **Application/container logs**: an unbounded default log driver (e.g.
   Docker's default `json-file`) can fill the disk on its own. Every
   long-running service needs an explicit rotating driver/policy, not the
   runtime default.

For both: record the commands to *check* current usage/config, not just the
target policy — a stale runbook with no verification command is a guess, not
an operational baseline.

## What goes in the produced runbook vs. elsewhere

- This baseline + guardrails + log policy → the ops runbook.
- Deployment steps (build, push, roll out, rollback) → a separate deployment
  runbook, cross-linked.
- Monitoring/alerting contracts and thresholds that page someone → a separate
  monitoring doc if the project has one, cross-linked rather than duplicated.

## Maintenance trigger

Revisit this document specifically when: the VPS is resized, the OS is
upgraded/replaced, service ownership changes, or a storage/logging policy
changes. A routine deploy or code change is not a trigger.
