# ADR-0004: Oracle Cloud Always Free ARM VM as deploy target

- **Status:** Accepted (approved by learner 2026-11-06)
- **Date:** 2026-11-06
- **Phase:** 6, 12

## Context
Low budget, but the full stack (Kafka, Postgres, Redis, 6 services, observability, k3s) needs about 8–16 GB RAM.
The local Mac has only 8 GB.

## Options considered
1. **Oracle Cloud Always Free (Ampere A1)**: up to 4 ARM cores / 24 GB RAM for $0. Caveats: card needed at sign-up, regional capacity shortages, idle-instance reclamation on free accounts (Pay-As-You-Go upgrade typically avoids that while staying free).
2. **Hetzner CAX21**: 4 vCPU ARM / 8 GB, ~€7–9/month, very reliable.
3. **AWS/GCP free tier**: ~1 GB RAM instances, too small; managed Kafka/K8s is expensive.
4. **Managed PaaS (Render/Railway/Fly)**: easy, but costs grow quickly with Kafka/Redis and you learn less infra.

## Decision
Oracle Always Free ARM as primary; Hetzner CAX21 as fallback. Phase 6: Docker Compose + Caddy (HTTPS).
Phase 12: k3s on the same VM. Also used as a remote Docker host from Phase 7 (see ADR-0005).

## Consequences
- Positive: $0, lots of RAM, ARM matches the M2 Mac and the GitHub ARM runners (one image architecture).
- Negative: single-node "cluster" (no real HA); dependence on Oracle's free-tier policy.
- Follow-ups: set a $1 budget alert; keep provisioning documented/scripted so we can move to Hetzner in under 1 hour.
