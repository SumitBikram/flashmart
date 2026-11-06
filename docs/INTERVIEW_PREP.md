# Interview Prep

Built up phase by phase. Write answers **in your own words**, then compare with the mentor's feedback.

## 1. Question bank by phase

> Added at the end of each phase. Format:
> **Q:** question · **My answer:** … · **Mentor feedback:** … · **Confidence:** 🔴 / 🟡 / 🟢

### Phase 0 – Setup
_(added when Phase 0 ends)_

### Phase J – Modern Java
_(added when Phase J ends)_

### Phase 1 – Spring Boot fundamentals
_(pending)_

### Phase 2 – Hibernate / JPA
_(pending)_

### Phase 3 – Security
_(pending)_

### Phase C – Concurrency
_(pending)_

### Phase 4 – Orders, Inventory, Payments
_(pending)_

### Phase 5 – Testing
_(pending)_

### Phase 6 – CI/CD & Deploy
_(pending)_

### Phase 6.5 – Modular monolith
_(pending)_

### Phase 7 – Microservices
_(pending)_

### Phase 8 – Kafka
_(pending)_

### Phase 9 – Redis
_(pending)_

### Phase 10 – Flash sale
_(pending)_

### Phase 11 – Observability
_(pending)_

### Phase 12 – Production
_(pending)_

## 2. STAR stories (from your war stories)

Take these from the "Bugs and real-world problems" section of [PROGRESS.md](PROGRESS.md).

| Story | Situation | Task | Action | Result (with numbers) |
|---|---|---|---|---|
| _(e.g. "Fixed N+1 on product listing")_ | | | | _(e.g. 101 queries → 2, p95 800 ms → 60 ms)_ |

## 3. System design: "Design a flash sale"

You build this answer in Phase 10. Structure:

1. Requirements and scale estimates (users, RPS, stock size)
2. Core challenge: oversell vs throughput
3. Design: rate limit → Redis atomic reservation → async order via Kafka → payment TTL → release
4. Failure modes: Redis down, Kafka lag, duplicate requests, bots
5. **Your measured numbers** from k6 (this is what sets you apart)

## 4. "Walk me through your project" (2-minute pitch)

_(draft after Phase 6, refine after Phase 10)_
