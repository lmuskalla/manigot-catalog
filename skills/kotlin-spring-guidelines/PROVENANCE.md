# Provenance

## kotlin-spring-guidelines

- **Upstream**: Kotlin/kotlin-backend-agent-skills - https://github.com/Kotlin/kotlin-backend-agent-skills
  - `.agents/skills/*/` — all 25 skills (fetched from `main`, 2026-10-09).
- **License**: MIT (see `LICENSE.txt`).
- **Adapted from the original**:
  - Merged the 25 upstream skills into one skill, so one description covers
    Kotlin + Spring work instead of 25 overlapping ones whose names (e.g.
    `test-suite-builder`, `observability-integrator`) didn't say Kotlin.
  - `SKILL.md` is new: one description, cross-cutting rules distilled from
    the upstream topics, and an index of topic files.
  - Each upstream skill's `SKILL.md` is now a topic file with its
    frontmatter removed, renamed as follows:
    `ci-cd-containerization-advisor` → `ci-cd.md`,
    `configuration-properties-profiles-kotlin-safe` → `configuration.md`,
    `dependency-conflict-resolver` → `dependency-conflicts.md`,
    `domain-decomposition-api-design-advisor` → `domain-design.md`,
    `error-model-validation-architect` → `error-model.md`,
    `gradle-kotlin-dsl-doctor` → `gradle.md`,
    `integration-resilience-engineer` → `integration-resilience.md`,
    `jackson-kotlin-serialization-specialist` → `jackson.md`,
    `java-kotlin-migration-assistant` → `java-to-kotlin.md`,
    `jpa-spring-data-kotlin-mapper` → `jpa.md`,
    `kotlin-idiomatic-refactorer-spring-aware` → `idiomatic-kotlin.md`,
    `kotlin-spring-proxy-compatibility` → `proxies.md`,
    `observability-integrator` → `observability.md`,
    `performance-concurrency-advisor` → `performance.md`,
    `production-incident-responder` → `incident-response.md`,
    `project-context-ingestion` → `project-context.md`,
    `schema-migration-planner` → `schema-migrations.md`,
    `spring-context-di-reasoning` → `context-and-di.md`,
    `spring-kotlin-code-review` → `code-review.md`,
    `spring-mvc-webflux-api-builder` → `http-apis.md`,
    `spring-security-configurator-auditor` → `security.md`,
    `stacktrace-log-triage` → `stacktrace-triage.md`,
    `test-suite-builder` → `testing.md`,
    `transaction-consistency-designer` → `transactions.md`,
    `upgrade-breaking-change-navigator` → `upgrades.md`.
  - Mentions of `project-context-ingestion` rewritten to links to
    `project-context.md`; "this skill" wording changed to "this topic".
  - Dropped each skill's `agents/openai.yaml` (vendor-specific UI metadata).
  - Otherwise body text verbatim.
