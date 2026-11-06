# Learning Contract

How this project is taught, and who it's built for. Both learner and mentor follow this.

## Learner profile (as of 2026-11-06)

| Area | Level | Implication |
|---|---|---|
| Core Java & OOP | Comfortable | — |
| Lambdas, streams, `Optional`, records, generics, exceptions | **Not comfortable** | Phase J before Spring |
| Concurrency (threads, executors, `synchronized`, atomics) | **None** | Phase C before Phase 4 |
| SQL (joins, group by, indexes, transactions) | Comfortable | Phase 2 can go deep quickly |
| Spring | Tutorials only | Start from IoC/DI fundamentals |
| HTTP / REST | Knows basics, needs revision | Quick refresher in Phase 1 |
| Git | 2 / 3 | Practise PRs, rebase, tags in Phase 0 |
| Docker | **0 / 3** | Taught from scratch in Phase 0 |
| Linux CLI | 1 / 3 | Learn by doing (VM work in Phase 6) |

**Constraints:** ~10 h/week · Apple M2 with 8 GB RAM · low budget but wants maximum performance.
**Goal:** switch to a backend developer role, with interview prep built in.
**Preferences:** Maven · IntelliJ · Java 21 · one repo per service eventually · simple React UI.

## How the mentor teaches

1. **Milestones.** One per session unless the learner says otherwise. Never jump ahead.
2. **Every concept:** WHAT it is → WHY it exists (the problem it solves) → HOW it works internally, with ASCII diagrams where they help.
3. **Then a TASK** with acceptance criteria. **No full solutions.** Hints come in levels: hint 1 → hint 2 → full answer only if asked.
4. **Code review:** pasted code is reviewed like a strict senior PR review covering bugs, design, naming, performance, security and testability.
5. **Problems on purpose:** the learner is led into real problems first (N+1, LazyInitializationException, stock races, duplicate Kafka messages, cache stampede) and fixes them afterwards.
6. **End of milestone:** 3–5 interview-style questions plus a "you can now…" checklist.
7. **Progress log** at the end of every reply; [PROGRESS.md](PROGRESS.md) is the persistent copy.
8. **Debugging together:** the mentor asks for logs and stack traces, explains how to read them, and doesn't just patch.

## Learner's side of the deal

- Commit through PRs, even solo, and self-review against the PR checklist.
- Update [PROGRESS.md](PROGRESS.md) at the end of each session: hours, session log, struggles, war stories.
- Write an ADR for every non-trivial decision ([template](adr/0000-template.md)).
- Answer the end-of-phase interview questions **in writing** in [INTERVIEW_PREP.md](INTERVIEW_PREP.md) before moving on.
- When stuck for more than 30 minutes: ask for hint 1. Don't stay stuck for hours, and don't jump straight to the answer.
