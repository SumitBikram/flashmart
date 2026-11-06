# Architecture Decision Records

An ADR captures **one** significant decision: the context, the options considered, the choice, and its consequences.
They're cheap to write and very useful in interviews ("why did you choose X?").

- Copy [0000-template.md](0000-template.md) to `NNNN-short-title.md`.
- Status lifecycle: `Proposed` → `Accepted` → (`Superseded by ADR-XXXX` | `Deprecated`).
- Never edit an accepted ADR's decision. Write a new one that supersedes it.

| ADR | Title | Status | Date |
|---|---|---|---|
| [0001](0001-java-21-lts.md) | Use Java 21 LTS | Accepted | 2026-11-06 |
| [0002](0002-maven-build-tool.md) | Use Maven as build tool | Accepted | 2026-11-06 |
| [0003](0003-monorepo-first-split-at-phase-7.md) | Single repo first, split per service at Phase 7 | Accepted | 2026-11-06 |
| [0004](0004-oracle-cloud-free-arm-deploy-target.md) | Oracle Cloud Always Free ARM VM as deploy target | Accepted | 2026-11-06 |
| [0005](0005-lightweight-local-docker-runtime.md) | OrbStack/Colima locally + remote Docker context | Accepted | 2026-11-06 |
| [0006](0006-modular-monolith-before-microservices.md) | Modular monolith before microservices | Accepted | 2026-11-06 |
