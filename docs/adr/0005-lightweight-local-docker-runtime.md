# ADR-0005: OrbStack/Colima locally + remote Docker context for heavy infra

- **Status:** Accepted (approved by learner 2026-11-06)
- **Date:** 2026-11-06
- **Phase:** 0, 7

## Context
Apple M2 with 8 GB RAM. macOS + IntelliJ use about 4–5 GB, leaving about 3 GB for containers. Docker Desktop's VM overhead is significant.

## Options considered
1. **Docker Desktop**: most common, but heavy; its license limits commercial use.
2. **OrbStack**: lightweight and fast on Apple Silicon, dynamic memory; free for personal use.
3. **Colima**: open source, lightweight; configure the VM size explicitly.

## Decision
OrbStack (or Colima if the learner prefers fully open-source). Rules:
- Small images (`postgres:18-alpine`, `apache/kafka-native`), `-Xmx256m` per service, Compose profiles.
- From Phase 7, heavy infra (Kafka, observability) runs on the cloud VM; the Mac uses `docker context` over SSH.

## Consequences
- Positive: the project stays workable on 8 GB; remote Docker contexts are a useful real-world skill.
- Negative: some phases need the VM to be up; network latency to the remote infra.
