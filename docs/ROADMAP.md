# Roadmap

**Status:** ✅ approved by learner on 2026-11-06.
**Pace:** ~10 h/week, starting Monday 2026-11-09. **Total:** ~300–370 h, about 8–9 months including buffer.

## Rules of the road

1. One milestone per session. Never skip ahead; if a phase overruns, the schedule moves, not the scope.
2. Every concept is covered as **what → why → how (internals)**, then a **task with acceptance criteria**.
3. Solutions come as hints in levels: hint 1 → hint 2 → full answer only on request.
4. Every phase ends with 3–5 interview questions, a "you can now…" checklist, a git tag, and an update to [PROGRESS.md](PROGRESS.md).
5. Tests are written from Phase 1 onwards. Phase 5 is the *deep dive*, not the first time you test.

## Changes from the original roadmap

| Change | Reason |
|---|---|
| **+ Phase J** (Modern Java bootcamp) | Lambdas, streams, `Optional`, records and generics are everywhere in Spring code |
| **+ Phase C** (Concurrency primer) before Phase 4 | Stock races and the flash sale are concurrency problems |
| Docker taught from zero in Phase 0 | Docker level is 0/3 |
| Flyway in Phase 1 (not Phase 2) | Avoid the `ddl-auto=update` habit |
| Basic CI from Phase 0/1 | Every PR gets checked from day one |
| **+ Phase 6.5** (Modular monolith) | Establish boundaries before splitting into microservices ([ADR-0006](adr/0006-modular-monolith-before-microservices.md)) |
| Repo split happens in Phase 7 | One repo per service is the goal; split once boundaries are known ([ADR-0003](adr/0003-monorepo-first-split-at-phase-7.md)) |
| Keycloak/OAuth2 moved to Phase 12 | Learn JWT properly first |
| **+ Phase 13** (React UI) | Portfolio demo |

## Timeline overview

| # | Phase | Hours | Weeks | Approx. done by |
|---|---|---|---|---|
| 0 | Setup | 10–12 | 1–2 | mid-Nov 2026 |
| J | Modern Java bootcamp | 12–15 | 2–3 | end Nov |
| 1 | Spring Boot fundamentals | 25–30 | 3 | mid-Dec |
| 2 | Hibernate deep dive | 25–30 | 3 | early Jan 2027 (holiday buffer) |
| 3 | Users & Security | 15–20 | 2 | mid-Jan |
| C | Concurrency primer | 8–10 | 1 | late Jan |
| 4 | Cart, Orders, Inventory, Payments | 25–30 | 3 | mid-Feb |
| 5 | Testing deep dive | 15–20 | 2 | end Feb |
| 6 | CI/CD v1 + first deploy | 12–15 | 1.5 | **early Mar** (start applying for jobs) |
| 6.5 | Modular monolith | 8–10 | 1 | mid-Mar |
| 7 | Microservices + repo split | 25–30 | 3 | early Apr |
| 8 | Kafka | 30–35 | 3.5 | end Apr |
| 9 | Redis | 15–20 | 2 | mid-May |
| 10 | Flash sale | 20–25 | 2.5 | end May |
| 11 | Observability | 15–20 | 2 | mid-Jun |
| 12 | Production | 25–30 | 3 | early Jul |
| 13 | React UI | 15–20 | 2 | **end Jul 2027** |

Outside these 10 h/week, once you start applying: 2–3 h/week of DSA practice (LeetCode-style).

---

## Phase 0 – Setup

**Goal:** a professional workspace where every change goes through Git and a PR, Postgres runs in Docker, and CI is green.

**Already done (2026-11-06):** JDK 21 (Homebrew), Maven 3.9.16, IntelliJ, Spring Initializr project (Boot 4.1.2).

**Steps**

| Step | Topic | Fixes | Est. |
|---|---|---|---|
| 0.1 | Git + GitHub: first commit, SSH key, public repo, ruleset on `main`, first PR | OI-2 | 2 h |
| 0.2 | Docker fundamentals by hand: OrbStack, images, containers, ports, volumes | OI-1 | 3 h |
| 0.3 | Maven multi-module restructure + package rename (via PR) | OI-3…OI-6 | 2 h |
| 0.4 | Compose: Postgres 18 with volume + healthcheck; connect from IntelliJ | — | 2 h |
| 0.5 | GitHub Actions CI + PR template; require CI to pass before merging | — | 2 h |

**Concepts**
- Docker from zero: image vs container, layers, registries, port mapping, volumes, networks, `docker compose`
- Why OrbStack/Colima instead of Docker Desktop on 8 GB ([ADR-0005](adr/0005-lightweight-local-docker-runtime.md))
- Git: trunk-based flow, short-lived branches, PRs, Conventional Commits, tags
- Maven: lifecycle (`validate → compile → test → package → verify → install`), wrapper, parent POM, multi-module, BOM
- Spring Initializr output: what each generated file is for

**Tasks (you implement)**
- Install a Docker runtime; run, inspect, stop and remove a container by hand before using Compose
- `git init`, a sensible `.gitignore`, first commit, GitHub repo, branch protection on `main`
- Restructure into a multi-module layout: root parent `pom.xml` and `services/monolith` (fixes OI-3 to OI-6)
- `infra/compose/compose.yaml` with Postgres 18, a named volume and a healthcheck; connect from IntelliJ's DB tool
- `.github/workflows/ci.yml` running `./mvnw -B verify` on PRs
- PR template plus a self-review checklist

**Deliberate problems:** data lost because there was no volume, a port conflict, a "works on my machine" difference between the Maven wrapper and local Maven.

**Exit criteria**
- [ ] `docker compose up -d` brings up a healthy Postgres; data survives `down`/`up`
- [ ] `./mvnw verify` passes locally and in GitHub Actions
- [ ] `main` is protected; at least one change merged through a PR
- [ ] You can explain image vs container vs volume without notes

## Phase J – Modern Java bootcamp

**Goal:** read and write idiomatic modern Java (the style Spring code uses) comfortably.

**Concepts:** functional interfaces and lambdas, method references, the Streams API (`map`/`filter`/`flatMap`/`collect`/`groupingBy`), `Optional` (correct use and anti-patterns), records, sealed interfaces plus pattern matching for `switch`, generics (bounded types, wildcards, PECS), checked vs unchecked exceptions, `equals`/`hashCode` contracts, immutability, `java.time`, `BigDecimal` for money.

**Tasks:** katas on e-commerce data (a list of products and orders), for example revenue per category, top N products, grouping orders by status, safely parsing prices. Unit-tested with JUnit.

**Deliberate problems:** `double` for money (rounding bugs), `Optional.get()` without checks, mutating a collection inside a stream, broken `equals`/`hashCode` inside a `HashSet`.

**Exit criteria:** solve 10 katas with streams and no loops; explain why `BigDecimal` is used for money and what PECS means.

## Phase 1 – Spring Boot fundamentals

**Goal:** a clean, validated, documented REST API for Products and Categories, backed by Postgres and Flyway.

**Concepts:** IoC/DI (constructor injection), the bean lifecycle and scopes, component scanning, how auto-configuration works (`@Conditional…`, `AutoConfiguration.imports`), profiles and externalized config, layered architecture, DTOs vs entities (records as DTOs), Bean Validation, `@RestControllerAdvice` with RFC 9457 `ProblemDetail`, pagination and sorting, OpenAPI/Swagger UI, Flyway basics, `.http` files, Actuator basics.

**Tasks:** CRUD for Category and Product (`/api/v1/...`), pagination and filtering, validation errors returned as ProblemDetail, Flyway `V1__init.sql`, unit tests for services and the first `@WebMvcTest`.

**Deliberate problems:** returning entities directly (infinite JSON recursion, data leaks), field injection that's hard to test, `contextLoads()` failing because there's no DB (OI-8), a hard-coded datasource password.

**Exit criteria:** API documented in Swagger UI; every error has a consistent shape; no entity crosses the controller boundary; CI green.

## Phase 2 – Hibernate / JPA deep dive

**Goal:** understand exactly what SQL Hibernate runs, and why.

**Concepts:** entity lifecycle states, the persistence context (first-level cache), dirty checking, flush modes, `@OneToMany`/`@ManyToOne`/`@ManyToMany` (owning side, `mappedBy`), cascade vs orphan removal, LAZY vs EAGER, fetch joins / `@EntityGraph` / DTO projections, `@Transactional` internals (proxies, propagation, `readOnly`, self-invocation), isolation levels and anomalies, optimistic (`@Version`) vs pessimistic locking, batch inserts, Flyway migration strategy (expand/contract).

**Deliberate problems:** **N+1** (count the SQL with logging), **LazyInitializationException**, open-in-view hiding problems, `@Transactional` skipped on self-invocation, a lost update from two concurrent edits.

**Exit criteria:** you can predict the SQL for a given repository call; the N+1 is fixed in at least two ways and you can compare them; OSIV is disabled on purpose.

## Phase 3 – Users & Security

**Concepts:** the Spring Security filter chain, `SecurityFilterChain` config, authentication vs authorization, BCrypt/Argon2, stateless JWT (structure, signing, expiry), refresh tokens and rotation, roles vs authorities, `@PreAuthorize`, CORS, the OWASP API Top 10 basics.

**Tasks:** register/login/refresh endpoints; `CUSTOMER` and `ADMIN` roles; only admins manage the catalog; users see only their own data.

**Deliberate problems:** an **IDOR** (user A reads user B's resource), a secret key committed to the repo, a too-long access-token TTL, CORS errors from a browser.

**Exit criteria:** security tests prove 401/403/200 per role; secrets come from env vars; you can draw the filter chain.

## Phase C – Concurrency primer

**Concepts:** threads, the Java Memory Model (visibility, happens-before), race conditions, `synchronized`, `volatile`, `ReentrantLock`, atomics, `ExecutorService`, `CompletableFuture`, thread-safe collections, deadlocks, **virtual threads** (Java 21), and why JVM locks don't work across multiple app instances.

**Tasks:** write a counter that loses updates under 1000 threads, then fix it three ways and benchmark them.

**Exit criteria:** explain a race condition and three ways to fix it; explain why `synchronized` can't protect stock when there are 2 app instances.

## Phase 4 – Cart, Orders, Inventory, Payments

**Concepts:** aggregate design, the order state machine (`PENDING → PAID → SHIPPED` / `CANCELLED`), inventory reservation vs deduction, idempotency keys, a mock payment gateway (latency, random failures), money handling, domain events inside the monolith.

**Deliberate problems:** **overselling stock** under a concurrent test (`stock = stock - 1` in Java), a **double charge** when the client retries, a long transaction holding locks while calling the payment gateway.

**Fix progression:** atomic SQL update (`UPDATE … WHERE stock >= ?`) → optimistic locking → pessimistic locking, with a measured comparison.

**Exit criteria:** a concurrent test with 200 buyers and 10 units sells exactly 10; payment retry is idempotent.

## Phase 5 – Testing deep dive

**Concepts:** the test pyramid, unit tests with Mockito (and when *not* to mock), slice tests (`@WebMvcTest`, `@DataJpaTest`), Testcontainers (Postgres) plus `@ServiceConnection`, reusable containers, test data builders, AssertJ, testing security, JaCoCo coverage gates, flaky-test hygiene.

**Deliberate problems:** an H2 vs Postgres behaviour difference, tests sharing state, a slow suite caused by context reloads.

**Exit criteria:** `./mvnw verify` runs unit and integration tests on real Postgres in CI; coverage is reported.

## Phase 6 – CI/CD v1 + first deploy

**Concepts:** multi-stage Dockerfile vs Buildpacks, layered jars, ARM images, GHCR, semantic versioning and tags, GitHub Actions (caching, matrix, secrets, environments), provisioning the Oracle VM (SSH keys, firewall, non-root user), Compose in production, Caddy reverse proxy with automatic HTTPS, Actuator health, a basic rollback (previous image tag).

**Deliberate problems:** a secret in an image layer, a container running as root, a migration that fails mid-deploy.

**Exit criteria:** merging to `main` builds, tests, pushes an image and deploys to the VM over HTTPS; rollback in under 5 minutes; README has a live URL. **→ Start applying for jobs.**

## Phase 6.5 – Modular monolith

**Concepts:** bounded contexts, Spring Modulith (module verification, `@ApplicationModuleListener`, the event publication registry), internal vs exposed APIs, documenting modules.

**Exit criteria:** the module structure test passes; modules communicate only through APIs or events; a diagram is generated.

## Phase 7 – Microservices + repo split

**Concepts:** DDD basics (aggregates, context mapping), database-per-service, Spring Cloud Gateway, service discovery (Eureka, then K8s DNS later), Config Server, `RestClient` / HTTP interfaces, Resilience4j (timeouts, retry with backoff and jitter, circuit breaker, bulkhead), correlation IDs, splitting repos with `git filter-repo`, publishing `flashmart-contracts` to GitHub Packages, reusable workflows.

**Deliberate problems:** a **cascading failure** when payment is slow, a retry storm, a distributed transaction that half-commits.

**Exit criteria:** 5 services plus a gateway running (heavy infra on the VM via a remote docker context); payment outage results in graceful degradation; each service repo has its own CI.

## Phase 8 – Kafka

**Concepts:** brokers, topics, partitions, offsets, KRaft, producer acks/idempotence, consumer groups and rebalancing, ordering per key, retries with backoff, DLQ topics, idempotent consumers (processed-message table), **Transactional Outbox**, **Saga** (choreography vs orchestration), compensation, schema evolution.

**Deliberate problems:** **duplicate messages** causing a double reservation, lost events from a dual write (DB commit + Kafka send), a poison pill blocking a partition, out-of-order events.

**Exit criteria:** an order → reserve stock → pay → confirm saga with compensation on failure; replaying a topic doesn't double-apply anything.

## Phase 9 – Redis

**Concepts:** data structures, cache-aside, TTL, invalidation strategies, Spring Cache abstraction, cart in Redis, rate limiting (fixed window vs token bucket; Gateway `RequestRateLimiter`), distributed locks (Redisson, lease time, fencing tokens), persistence (RDB vs AOF).

**Deliberate problems:** a **cache stampede** on a hot product, a stale cache after an update, a lock that expires mid-work.

**Exit criteria:** p95 latency for the catalog measurably lower; stampede mitigated (request coalescing / jittered TTL); rate limit returns 429.

## Phase 10 – Flash sale

**Concepts:** traffic shaping, stock pre-loaded into Redis, an **atomic Lua** check-and-decrement, reservations with a TTL, a Kafka buffer between "won the slot" and "create the order" (async 202 + polling), one purchase per user, bot mitigation, virtual threads, hot keys, load testing with **k6** (scenarios, thresholds).

**Deliberate problems:** the DB collapsing at a few thousand RPS with the Phase 4 approach, an oversell between Redis and the DB, reservations never released.

**Exit criteria:** k6 at the target RPS shows **zero oversell**, a bounded p99, and the DB protected; a write-up compares before and after.

## Phase 11 – Observability

**Concepts:** structured JSON logs and MDC correlation, the RED/USE methods, Micrometer → Prometheus, custom business metrics (orders/sec, sold-out rate), OpenTelemetry tracing across HTTP and Kafka, Grafana dashboards, SLOs and alerting, the `grafana/otel-lgtm` all-in-one stack on the VM.

**Exit criteria:** one trace shows gateway → order → Kafka → inventory; a dashboard covers the flash sale; an alert fires on an error-rate SLO breach.

## Phase 12 – Production

**Concepts:** k3s on the VM, Deployments/Services/Ingress, liveness/readiness/startup probes, resource requests and limits, ConfigMaps and Secrets (+ Sealed Secrets), HPA, rolling vs blue/green, zero-downtime DB migrations, graceful shutdown, a GitOps repo (`flashmart-platform`), per-service CI/CD v2, a rollback runbook, Keycloak (OAuth2/OIDC) replacing hand-rolled JWT, backups.

**Exit criteria:** deploy without dropped requests (proven with k6 during a rollout); one-command rollback; secrets not in plain text in Git.

## Phase 13 – React UI

**Concepts:** Vite + React + TypeScript, calling the API through the gateway, auth token handling, CORS, a flash-sale countdown and the "you're in the queue" polling UX.

**Exit criteria:** the deployed UI lets a user browse, log in, add to cart, check out and join a flash sale.

## Optional extensions

OpenSearch product search · Notification service (email via Kafka) · Spring Authorization Server · Chaos testing · GraalVM native images.
