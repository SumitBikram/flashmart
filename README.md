# FlashMart: E-commerce Platform with Flash Sales

> Working name: **FlashMart**. The folder and Maven artifact are still called `e-commerce`. Renaming is an open decision; see [PROGRESS.md](docs/PROGRESS.md).

A production-grade e-commerce backend built step by step as a learning project. It starts as a
Spring Boot monolith and grows into event-driven microservices. The headline feature is a
**high-concurrency flash sale**.

## Documentation map

| Document | What it holds | Update it when… |
|---|---|---|
| [docs/PROGRESS.md](docs/PROGRESS.md) | **Source of truth**: current phase, phase tracker, session log, open issues, struggled concepts | Every session |
| [docs/ROADMAP.md](docs/ROADMAP.md) | All phases with topics, deliberate problems, deliverables and exit criteria | Scope changes |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Target architecture, flash-sale flow, infra topology, repo layout | Design changes |
| [docs/TECH_STACK.md](docs/TECH_STACK.md) | Versions (verified vs assumed), local tooling, memory budget | Upgrades, new tools |
| [docs/LEARNING_CONTRACT.md](docs/LEARNING_CONTRACT.md) | How the mentoring works, learner profile and constraints | Rarely |
| [docs/INTERVIEW_PREP.md](docs/INTERVIEW_PREP.md) | Interview questions per phase, STAR stories, system-design notes | End of each phase |
| [docs/adr/](docs/adr/README.md) | Architecture Decision Records: *why* we chose what we chose | Any significant decision |

## Resuming a session

Paste the **"Current status"** block from [docs/PROGRESS.md](docs/PROGRESS.md) into a new
mentoring session. That block is designed to be self-contained.

## Quick facts

- **Stack:** Java 21 · Spring Boot 4.1 · Maven · PostgreSQL · Kafka · Redis · Docker · GitHub Actions · k3s
- **Machine:** Apple M2, 8 GB RAM (see the memory strategy in [ARCHITECTURE.md](docs/ARCHITECTURE.md#4-infrastructure-topology-8-gb-mac--free-cloud-vm))
- **Deploy target:** Oracle Cloud Always Free ARM VM (see [ADR-0004](docs/adr/0004-oracle-cloud-free-arm-deploy-target.md))
- **Pace:** ~10 h/week · started 2026-11-06 · target finish ≈ end of July 2027
