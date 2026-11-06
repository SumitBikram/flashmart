# Progress Log

> **Source of truth.** Update it at the end of every session. To resume with the mentor, paste the
> **Current status** block below.

---

## Current status (paste this into a new session)

```
PROJECT:            FlashMart (e-commerce + flash sale), learning project
DATE STARTED:       2026-11-06
LEARNER:            Core Java OK; weak on lambdas/streams/Optional/records/generics & concurrency;
                    SQL strong; Spring = tutorials; Git 2/3, Docker 0/3, Linux 1/3; REST needs revision
CONSTRAINTS:        10 h/week · Apple M2 8 GB RAM · low budget · goal = job switch + interview prep
STACK:              Java 21 · Spring Boot 4.1.2 · Maven 3.9 · IntelliJ · Postgres 18 · Kafka 4 · Redis 8
ROADMAP ORDER:      0 → J → 1 → 2 → 3 → C → 4 → 5 → 6 → 6.5 → 7 → 8 → 9 → 10 → 11 → 12 → 13
MILESTONES DONE:    none
CURRENT MILESTONE:  Phase 0 (Setup), started 2026-11-06
PHASE 0 STEPS:      0.1 Git+GitHub 🟨 · 0.2 Docker basics 🟨 · 0.3 Multi-module+rename ⬜ · 0.4 Compose Postgres ⬜ · 0.5 CI ⬜
PHASE 0 STATE:      JDK 21 ✅ Maven ✅ IntelliJ ✅ Initializr project ✅ git init (main) ✅ | first commit ❌ Docker ❌ multi-module ❌
OPEN ISSUES:        see "Open issues" table (OI-1 … OI-8 open; OI-9 closed)
STRUGGLED WITH:     n/a yet (watch: modern Java syntax, concurrency, Docker)
DECISIONS:          D-1…D-5 approved 2026-11-06 (roadmap, Oracle Free, single repo→split P7, UI P13, package flashmart)
PENDING DECISIONS:  none (D-6 public repo ✅ github.com/SumitBikram/flashmart)
```

---

## Phase tracker

Legend: ⬜ not started · 🟨 in progress · ✅ done · ⏸ paused

| # | Phase | Status | Est. hours | Actual hours | Started | Finished | Tag |
|---|---|---|---|---|---|---|---|
| 0 | Setup (tools, Git, Docker, Postgres, CI) | 🟨 in progress | 10–12 | 0 | 2026-11-06 | — | `v0.0.0` |
| J | Modern Java bootcamp | ⬜ | 12–15 | — | — | — | — |
| 1 | Spring Boot fundamentals + REST | ⬜ | 25–30 | — | — | — | `v0.1.0` |
| 2 | Hibernate / JPA deep dive | ⬜ | 25–30 | — | — | — | `v0.2.0` |
| 3 | Users & Security (JWT) | ⬜ | 15–20 | — | — | — | `v0.3.0` |
| C | Concurrency primer | ⬜ | 8–10 | — | — | — | — |
| 4 | Cart, Orders, Inventory, Payments | ⬜ | 25–30 | — | — | — | `v0.4.0` |
| 5 | Testing deep dive | ⬜ | 15–20 | — | — | — | `v0.5.0` |
| 6 | CI/CD v1 + first deploy | ⬜ | 12–15 | — | — | — | `v0.6.0` |
| 6.5 | Modular monolith | ⬜ | 8–10 | — | — | — | `v0.6.5` |
| 7 | Microservices + repo split | ⬜ | 25–30 | — | — | — | `v0.7.0` |
| 8 | Kafka (Saga, Outbox, DLQ) | ⬜ | 30–35 | — | — | — | `v0.8.0` |
| 9 | Redis | ⬜ | 15–20 | — | — | — | `v0.9.0` |
| 10 | Flash sale | ⬜ | 20–25 | — | — | — | `v0.10.0` |
| 11 | Observability | ⬜ | 15–20 | — | — | — | `v0.11.0` |
| 12 | Production (k3s, CI/CD v2) | ⬜ | 25–30 | — | — | — | `v1.0.0` |
| 13 | React UI | ⬜ | 15–20 | — | — | — | `v1.1.0` |
| | **Total** | | **300–370** | **0** | | | |

Job-switch checkpoint: **start applying after Phase 6** (≈ March 2027).

---

## Pending decisions

| ID | Decision | Mentor recommendation | Status |
|---|---|---|---|
| D-1 | Approve refined roadmap (Phase 0 → J → … → 13) | Approve | ✅ approved 2026-11-06 |
| D-2 | Deploy target | Oracle Cloud Always Free ARM; Hetzner CAX21 as fallback ([ADR-0004](adr/0004-oracle-cloud-free-arm-deploy-target.md)) | ✅ approved 2026-11-06 |
| D-3 | Repo strategy | Single repo now, split per service at Phase 7 ([ADR-0003](adr/0003-monorepo-first-split-at-phase-7.md)) | ✅ approved 2026-11-06 |
| D-4 | React UI timing | Phase 13 (end); optional mini page after Phase 6 | ✅ approved 2026-11-06 |
| D-5 | Project and package name | Rename `com.sumitbm.e_commerce` → `com.sumitbm.flashmart` in Phase 0 step 0.3 (see OI-3) | ✅ approved 2026-11-06 |
| D-6 | GitHub repo visibility | **Public**: portfolio value, free branch rulesets and free ARM runners. Requires strict "no secrets in Git" discipline | ✅ public: github.com/SumitBikram/flashmart |

---

## Open issues

Found by inspecting the repo on 2026-11-06. These are Phase 0 work items for **you** to fix.

| ID | Issue | Why it matters | Fix in |
|---|---|---|---|
| OI-1 | No Docker runtime installed (`docker: command not found`) | Postgres, Testcontainers and everything later need it | Phase 0 |
| OI-2 | Folder is not a Git repository. 🟨 `git init` on `main`; remote `origin` added (HTTPS → switch to SSH); no commit yet | No history, no PRs, no CI | Phase 0 |
| OI-3 | Package is `com.sumitbm.e_commerce` (Initializr replaced the invalid `-` with `_`) | Unidiomatic Java package name, and it gets copied everywhere | Phase 0 |
| OI-4 | Single-module project; target layout is a Maven multi-module (`services/monolith`) | Restructuring later is more painful | Phase 0 |
| OI-5 | `pom.xml` has empty `<name/>`, `<description/>`, `<url/>`, `<scm/>`, `<licenses/>`, `<developers/>` overrides | Noise; fill in or keep deliberately (read `HELP.md` first) | Phase 0 |
| OI-6 | `HELP.md` is Initializr boilerplate | Clutter only: it's already in `.gitignore`, so it won't be committed. Delete the local file when convenient | Phase 0 |
| OI-9 | ✅ **Closed 2026-11-06** (4th review). `.gitignore` now uses `*.log`, `.env`, `.env.*`, `!.env.example`, verified in subfolders. Optional nits left: `### Secrets` missing closing `###`, double blank line. Reminder: re-stage before the first commit (staged copy was out of date) | — | Done |
| OI-7 | `postgresql` driver is declared but there is no JDBC/JPA starter or datasource config | Harmless now; it will matter in Phase 1 | Phase 1 |
| OI-8 | `contextLoads()` test will need a running DB once JPA is added | You'll hit this deliberately, and it motivates Testcontainers | Phase 1 / 5 |

---

## Session log

| # | Date | Phase | Hours | What happened | Next step |
|---|---|---|---|---|---|
| 1 | 2026-11-06 | Planning | ~1 | Collected background; refined roadmap (added Phase J, Phase C, Phase 6.5, Phase 13); chose Java 21, Maven, IntelliJ; 8 GB RAM strategy; proposed Oracle Free ARM + monorepo-then-split; created docs. Found existing Initializr project (Boot 4.1.2). | Approve roadmap (D-1…D-5) → start Phase 0 |
| 2 | 2026-11-06 | 0 | — | Roadmap approved (D-1…D-5); ADR-0003…0006 accepted. Phase 0 started: lessons and tasks given for step 0.1 (Git + GitHub) and step 0.2 (Docker fundamentals). | Complete 0.1 + 0.2, report outputs → step 0.3 |

---

## Concepts I struggled with

> Format: concept · phase · what confused me · what made it click · revisit date

_(none yet)_

---

## Bugs and real-world problems I hit (my "war stories")

> These become your STAR stories in [INTERVIEW_PREP.md](INTERVIEW_PREP.md).
> Format: symptom → how I debugged it → root cause → fix → what I learned

_(none yet)_
