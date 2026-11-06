# ADR-0001: Use Java 21 LTS

- **Status:** Accepted
- **Date:** 2026-11-06
- **Phase:** 0

## Context
We need a Java LTS release that Spring Boot 4.x supports and that hiring managers expect.
Java 25 is the newest LTS; Java 21 is the one most job descriptions ask for.

## Options considered
1. **Java 21 LTS**: what most job ads and production systems use; has virtual threads, records, pattern matching for `switch`, sequenced collections. Already installed (21.0.12).
2. **Java 25 LTS**: newest features, but less common in jobs today.

## Decision
Java 21, the learner's choice. It matches the job market and has every feature this roadmap needs (virtual threads for Phase 10).

## Consequences
- Positive: interview-relevant; no learning time spent on very new features.
- Negative: a later upgrade to 25 will be needed (that upgrade is a good exercise in itself).
