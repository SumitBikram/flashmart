# Tech Stack & Versions

Legend: ✅ verified on this machine/repo · 📌 to pin when the phase starts (check start.spring.io / official release notes) · 🔗 managed by the Spring Boot BOM (don't set the version yourself)

## Machine

| Item | Value | Verified |
|---|---|---|
| Hardware | Apple M2 (arm64), **8 GB RAM** | ✅ 2026-11-06 |
| IDE | IntelliJ IDEA | ✅ installed |
| Git | 2.54.0 (Apple Git) | ✅ |
| JDK | OpenJDK **21.0.12.1** (Homebrew `openjdk@21`) | ✅ |
| Maven | Wrapper pins **3.10.0** (`.mvn/wrapper/maven-wrapper.properties`, wrapper 3.3.4, only-script). Homebrew `mvn` is 3.9.16, so **always use `./mvnw`** | ✅ |
| Docker runtime | **not installed**. Plan: OrbStack (or Colima) | ❌ Phase 0 |

## Application stack

| Area | Choice | Version | Status | Phase |
|---|---|---|---|---|
| Language | Java LTS | 21 | ✅ in `pom.xml` | 0 |
| Framework | Spring Boot | **4.1.2** | ✅ in `pom.xml` | 0 |
| Core | Spring Framework | 7.x | 🔗 | 1 |
| Persistence | Spring Data JPA + Hibernate | 7.x | 🔗 | 1–2 |
| DB | PostgreSQL | 18 (`postgres:18-alpine`) | 📌 | 0 |
| Migrations | Flyway | | 🔗 | 1 |
| API docs | springdoc-openapi | Boot-4-compatible release | 📌 | 1 |
| Security | Spring Security + JWT (Nimbus via resource server) | | 🔗 | 3 |
| Identity (later) | Keycloak | latest | 📌 | 12 |
| Testing | JUnit Jupiter, Mockito, AssertJ | | 🔗 | 1, 5 |
| Testing | Testcontainers | 2.x | 🔗 | 5 |
| Modular monolith | Spring Modulith | Boot-4-compatible | 📌 | 6.5 |
| Microservices | Spring Cloud (Gateway, Config, Netflix Eureka) | release train matching Boot 4.1 | 📌 | 7 |
| Resilience | Resilience4j | | 📌 | 7 |
| Messaging | Apache Kafka (KRaft) + Spring for Apache Kafka | 4.x / 🔗 | 📌 | 8 |
| Cache | Redis + Spring Data Redis (+ Redisson for locks) | 8.x | 📌 | 9 |
| Load testing | k6 | latest | 📌 | 10 |
| Observability | Micrometer, OpenTelemetry, Prometheus, Grafana, Tempo, Loki (`grafana/otel-lgtm`) | | 📌 | 11 |
| Orchestration | k3s | latest stable | 📌 | 12 |
| CI/CD | GitHub Actions (ARM runners) + GHCR | — | — | 0, 6, 12 |
| Frontend | Vite + React + TypeScript | latest | 📌 | 13 |

**Tip:** run `./mvnw dependency:tree` to see the exact versions the Boot BOM picked.
Override a BOM version only with a written reason.

## Infra / hosting

| Item | Choice | Cost | Status |
|---|---|---|---|
| Local containers | OrbStack (free for personal use) or Colima (OSS) | $0 | Accepted ([ADR-0005](adr/0005-lightweight-local-docker-runtime.md)) |
| Cloud VM | Oracle Cloud Always Free, Ampere A1 (up to 4 OCPU / 24 GB) | $0 | Accepted ([ADR-0004](adr/0004-oracle-cloud-free-arm-deploy-target.md)), re-check terms in Phase 6 |
| Fallback VM | Hetzner CAX21 (4 vCPU ARM / 8 GB) | ~€7–9/mo, check current price | Fallback |
| Image registry | GitHub Container Registry | $0 | Phase 6 |
| TLS / domain | Caddy + Let's Encrypt; sslip.io/DuckDNS → own domain later (~$10/yr) | $0 | Phase 6 |
