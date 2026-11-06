# Architecture

> Describes the **target** (end of Phase 12) and how we get there. Every significant choice is
> recorded in an [ADR](adr/README.md).

## 1. Evolution

```
Phase 1–6            Phase 6.5                     Phase 7+
┌──────────────┐     ┌─────────────────────────┐   ┌────────┐ ┌────────┐ ┌────────┐
│   Monolith   │ ──▶ │  Modular monolith       │──▶│catalog │ │ order  │ │ ...    │
│ (packages by │     │  (Spring Modulith:      │   │service │ │service │ │service │
│   feature)   │     │   verified boundaries,  │   └────────┘ └────────┘ └────────┘
└──────────────┘     │   in-process events)    │     separate DBs, Kafka events
   1 Postgres        └─────────────────────────┘
```

### Monolith package layout (Phases 1–6.5)

Organized by feature, not by layer:

```
com.sumitbm.flashmart            (after renaming from com.sumitbm.e_commerce, OI-3)
├── catalog/      products, categories
├── identity/     users, auth, JWT
├── cart/
├── order/
├── inventory/
├── payment/      mock gateway
└── shared/       error handling, config, common types (keep small!)
```

Each feature package contains its own `api` (controllers, DTOs), `domain` (entities, services) and `infra` (repositories, clients).

## 2. Target architecture (end of Phase 12)

```
                         ┌──────────────┐
 React UI / k6 ─────────▶│ API Gateway  │── JWT validation, rate limit (Redis), routing
                         └──────┬───────┘
          ┌──────────────┬──────┼─────────────┬───────────────┐
          ▼              ▼      ▼             ▼               ▼
   ┌────────────┐ ┌───────────┐ ┌───────────┐ ┌────────────┐ ┌───────────┐
   │  identity  │ │ catalog   │ │  order    │ │ inventory  │ │ payment   │
   │ (users/JWT │ │ products  │ │ cart+saga │ │ stock,     │ │ mock PSP, │
   │ →Keycloak) │ │ categories│ │ orchestr. │ │ flash-sale │ │ idempot.  │
   └─────┬──────┘ └────┬──────┘ └─────┬─────┘ └─────┬──────┘ └─────┬─────┘
         │PG           │PG + Redis    │PG + outbox  │PG + Redis Lua │PG + outbox
         │             │  cache       │             │  (stock)      │
         │             │              ▼             ▼               ▼
         │             │      ┌─────────────────────────────────────────┐
         │             └─────▶│        Kafka (KRaft) + DLQ topics       │
         │                    └─────────────────────────────────────────┘
   ── cross-cutting ─────────────────────────────────────────────────────
   OpenTelemetry → Tempo (traces) │ Prometheus → Grafana (metrics/alerts)
   Loki (logs)                    │ GitHub Actions → GHCR → k3s (rolling)
```

### Service responsibilities and data ownership

| Service | Owns | Database | Publishes (planned) | Consumes (planned) |
|---|---|---|---|---|
| gateway | routing, auth check, rate limit | Redis (rate limit) | — | — |
| identity | users, credentials, roles | `identity_db` | `user.registered` | — |
| catalog | products, categories, prices | `catalog_db` + Redis cache | `product.updated` | — |
| order | carts, orders, saga state | `order_db` (+ outbox) | `order.created`, `order.confirmed`, `order.cancelled` | `stock.reserved`, `stock.rejected`, `payment.completed`, `payment.failed` |
| inventory | stock, reservations, flash-sale stock | `inventory_db` + Redis | `stock.reserved`, `stock.rejected`, `stock.released` | `order.created`, `order.cancelled` |
| payment | payments, idempotency keys | `payment_db` (+ outbox) | `payment.completed`, `payment.failed` | `stock.reserved` |

Rule: **no service reads another service's database.** Separate databases can live on one Postgres instance to save RAM.

## 3. Flash-sale request path (Phase 10)

```
POST /api/v1/flash-sales/{id}/purchase   (Idempotency-Key header)
  │
  ├─▶ Gateway: auth + per-user rate limit (Redis)  ──▶ 429 if exceeded
  │
  ├─▶ inventory-service: Redis Lua (atomic):
  │       if already_bought(user) → return DUPLICATE
  │       if stock <= 0          → return SOLD_OUT          ──▶ 409, DB never touched
  │       decr stock; add reservation(user, ttl=10m)
  │
  ├─▶ publish flash.reserved → Kafka            ──▶ 202 Accepted + reservationId
  │
  ├─▶ order-service consumes → creates order (idempotent) → saga continues
  │
  └─▶ payment timeout / failure → stock.released → Lua re-increments stock
```

## 4. Infrastructure topology (8 GB Mac + free cloud VM)

```
   ┌──────── Mac M2 (8 GB) ──────────┐          ┌──── Oracle ARM VM (24 GB, $0) ────────┐
   │ IntelliJ                        │  docker  │ Phase 6:  Compose + Caddy (HTTPS)      │
   │ 1–2 services you're editing     │ context  │ Phase 7+: Kafka, Postgres, Redis       │
   │ OrbStack: Postgres+Redis (≤P6)  │ ───────▶ │ Phase 11: grafana/otel-lgtm stack      │
   └─────────────────────────────────┘   SSH    │ Phase 12: k3s cluster (all services)   │
                ▲                               └───────────────▲────────────────────────┘
                │ git push                                      │ deploy
        ┌───────┴──────────┐   build/test (ARM runner)          │
        │  GitHub Actions  │ ── push arm64 image → GHCR ────────┘
        └──────────────────┘
```

### Local memory budget

| Item | Approx. RAM |
|---|---|
| macOS + IntelliJ (IDE heap ≤ 2 GB) | 4–5 GB |
| Docker VM (OrbStack/Colima, capped) | ≤ 3 GB |
| ├ Postgres 18-alpine | ~100 MB |
| ├ Redis 8 | ~20 MB |
| ├ Spring Boot service (`-Xmx256m`) | ~350 MB each |
| └ Kafka (`apache/kafka-native`, single broker) | ~300 MB (on the VM from Phase 7) |

Rules: use Compose **profiles** so only what you need runs; from Phase 7, heavy infra runs on the VM.

## 5. Repository layout

### Now → Phase 6.5 (single repo, Maven multi-module)

```
e-commerce/   (→ flashmart)
├── .github/workflows/ci.yml
├── docs/            ← these docs (ADRs, roadmap, progress)
├── infra/
│   ├── compose/     compose.yaml (+ profiles)
│   └── observability/
├── services/
│   └── monolith/    the current Initializr project moves here in Phase 0
├── load-tests/k6/
├── http/            .http request files
├── pom.xml          parent POM (packaging=pom)
└── README.md
```

### Phase 7+ (one repo per service, in a GitHub organization)

```
flashmart-gateway            flashmart-contracts   (event schemas → GitHub Packages)
flashmart-identity-service   flashmart-platform    (compose, k8s, observability, docs → GitOps)
flashmart-catalog-service    flashmart-web         (React UI)
flashmart-order-service      .github               (shared reusable CI workflows)
flashmart-inventory-service
flashmart-payment-service
```

At the split, `docs/` moves to `flashmart-platform`. Services are extracted with `git filter-repo` to keep their history.
