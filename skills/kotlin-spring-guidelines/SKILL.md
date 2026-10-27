---
name: kotlin-spring-guidelines
description: Use for any task in a Kotlin or Kotlin + Spring Boot backend — writing, reviewing, testing, debugging, building, or upgrading it. Covers Gradle Kotlin DSL builds and dependency conflicts, bean wiring and startup failures, @Transactional/@Cacheable/@Async proxy pitfalls, JPA/Hibernate entities, Jackson serialization, MVC/WebFlux REST APIs, validation and error payloads, Spring Security, @ConfigurationProperties and profiles, transactions and idempotency, resilient integrations, Flyway/Liquibase migrations, observability, performance, CI/CD and containers, stack-trace triage, incidents, Java-to-Kotlin migration, and Spring Boot upgrades.
---

# Kotlin + Spring Guidelines

One entry point for Kotlin + Spring backends. This file holds the rules that
apply to every change and an index of topic files; open only the topics the
task touches.

## Always

1. Start with [project-context.md](project-context.md) unless the project's
   stack is already established in this session: build and config files
   before business code; record Kotlin, Spring Boot, Gradle, and JDK versions,
   compiler plugins, MVC vs WebFlux, and the persistence technology.
2. Treat the project's real versions and conventions as hard limits. No APIs
   from a different Spring Boot major, no `javax.*`/`jakarta.*` mix-ups, no
   dependency versions that fight the Boot BOM or version catalog.
3. Kotlin classes are final: AOP annotations (`@Transactional`, `@Cacheable`,
   `@Async`, ...) need `plugin.spring`, and JPA entities need `plugin.jpa` — see
   [proxies.md](proxies.md).
4. No `data class` JPA entities; see [jpa.md](jpa.md).
5. Prefer the smallest change that fixes the problem and can be verified with
   a test or a build.

## Topics

| Topic | Open when |
|-------|-----------|
| [project-context.md](project-context.md) | First look at a repository: versions, modules, stack, risk markers |
| [gradle.md](gradle.md) | `build.gradle.kts`/`settings.gradle.kts` failing, plugin or toolchain incompatibility |
| [dependency-conflicts.md](dependency-conflicts.md) | `NoSuchMethodError`, `ClassNotFoundException`, linkage errors, version drift |
| [context-and-di.md](context-and-di.md) | Context startup failures, missing/duplicate beans, circular dependencies, auto-config conditions |
| [proxies.md](proxies.md) | `@Transactional`, `@Cacheable`, `@Async`, method security, or retry appear to do nothing |
| [configuration.md](configuration.md) | `@ConfigurationProperties` binding, profiles, environment overrides, secrets |
| [http-apis.md](http-apis.md) | Building or changing REST endpoints, DTOs, MVC vs WebFlux |
| [error-model.md](error-model.md) | Error payloads, exception mapping, status codes, `@ControllerAdvice`, validation |
| [jackson.md](jackson.md) | JSON (de)serialization failures, nullability/defaults, date-time or enum formats, polymorphism |
| [jpa.md](jpa.md) | Entities, relationships, fetch plans, repository queries, lazy loading, N+1 |
| [transactions.md](transactions.md) | Transaction boundaries, rollback, idempotency, locking, outbox, multi-repository writes |
| [schema-migrations.md](schema-migrations.md) | Flyway/Liquibase changes, zero-downtime schema evolution |
| [integration-resilience.md](integration-resilience.md) | HTTP clients, messaging, scheduled jobs: timeouts, retries, circuit breakers, DLQs |
| [security.md](security.md) | Spring Security filter chains, JWT/OAuth2 resource servers, method security, CORS, CSRF |
| [testing.md](testing.md) | Unit, slice, and integration tests; Testcontainers; test strategy |
| [idiomatic-kotlin.md](idiomatic-kotlin.md) | Cleaning up Java-flavored Kotlin without breaking Spring behavior |
| [java-to-kotlin.md](java-to-kotlin.md) | Converting Java classes to Kotlin, replacing Lombok |
| [domain-design.md](domain-design.md) | Bounded contexts, module/service boundaries, API contracts for a new feature or redesign |
| [observability.md](observability.md) | Logs, metrics, tracing, health endpoints, SLOs and alerts |
| [performance.md](performance.md) | Slow endpoints, pool saturation, coroutine/thread issues, throughput and latency |
| [stacktrace-triage.md](stacktrace-triage.md) | Diagnosing a failure from a stack trace or logs |
| [incident-response.md](incident-response.md) | A live production incident: error spikes, latency, bad deploy |
| [ci-cd.md](ci-cd.md) | Dockerfiles, CI workflows, Kubernetes manifests, rollout strategy |
| [upgrades.md](upgrades.md) | Spring Boot/Framework, Kotlin, Gradle, or JDK major upgrades; `javax` → `jakarta` |
| [code-review.md](code-review.md) | Reviewing a Kotlin + Spring PR, diff, or design change |
