# ADR-0003: Single repo first, split into one repo per service at Phase 7

- **Status:** Accepted (approved by learner 2026-11-06)
- **Date:** 2026-11-06
- **Phase:** 0 → 7

## Context
The learner wants one repo per service in the end. Before Phase 7 there is only one deployable
(the monolith) and the service boundaries are not yet known. Time is limited (10 h/week).

## Options considered
1. **Polyrepo from day 1**: splits before the boundaries are known; lots of repo/CI overhead too early.
2. **Monorepo forever**: simplest, but doesn't match the learner's goal.
3. **Monorepo until Phase 6.5, then split per service at Phase 7**: boundaries are proven by Spring Modulith first.

## Decision
Option 3. At Phase 7, extract each service with `git filter-repo` (keeping its history) into a GitHub
organization: one repo per service, plus `flashmart-contracts`, `flashmart-platform` (GitOps/infra/docs),
`flashmart-web` and `.github` (reusable workflows).

## Consequences
- Positive: learns both styles; a real migration story for interviews; time goes into backend learning early on.
- Negative: shared contracts need versioning and publishing (GitHub Packages); cross-repo changes need coordination.
- Follow-up: write ADR on contract versioning at Phase 7.
