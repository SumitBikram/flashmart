# ADR-0002: Use Maven as build tool

- **Status:** Accepted
- **Date:** 2026-11-06
- **Phase:** 0

## Context
We need a build tool for a multi-module project that later becomes several repos.

## Options considered
1. **Maven**: declarative and explicit, very common in enterprise jobs, simple multi-module setup and BOM imports.
2. **Gradle (Kotlin DSL)**: faster builds and more flexible, but more "magic" to learn on top of Spring.

## Decision
Maven, with the Maven wrapper (`mvnw`) committed so local and CI builds use the same version.

## Consequences
- Positive: predictable lifecycle, easy to read, and most tutorials/docs show Maven.
- Negative: verbose XML; slower than Gradle on large builds (not a problem at our size).
