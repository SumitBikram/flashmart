# ADR-0006: Modular monolith (Spring Modulith) before microservices

- **Status:** Accepted (approved by learner 2026-11-06)
- **Date:** 2026-11-06
- **Phase:** 6.5

## Context
Splitting a tangled monolith into services mostly produces a distributed monolith. Boundaries should be proven first.

## Options considered
1. **Split directly from the Phase 6 monolith**: fast, but hidden coupling surfaces as network calls.
2. **Enforce module boundaries first with Spring Modulith**: verified boundaries and event-based communication, then extract.

## Decision
Option 2. Each feature package becomes a verified module; cross-module calls go only through exposed APIs or domain events.

## Consequences
- Positive: Phase 7 becomes about distributed-systems problems, not untangling code; this is the "monolith-first" pattern used in industry.
- Negative: about 8–10 h added to the roadmap.
