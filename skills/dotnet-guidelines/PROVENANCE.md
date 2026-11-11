# Provenance

## dotnet-guidelines

- **Upstream**: dotnet/skills - https://github.com/dotnet/skills
  (fetched from `main`, 2026-10-09):
  - `plugins/dotnet-diag/skills/analyzing-dotnet-performance/`
  - `plugins/dotnet-msbuild/skills/binlog-failure-analysis/`
  - `plugins/dotnet-advanced/skills/csharp-scripts/`
  - `plugins/dotnet-aspnetcore/skills/dotnet-webapi/`
  - `plugins/dotnet-diag/skills/microbenchmarking/`
  - `plugins/dotnet-data/skills/optimizing-ef-core-queries/`
  - `plugins/dotnet-advanced/skills/vectorization/`
- **License**: MIT (see `LICENSE.txt`).
- **Adapted from the original**:
  - Merged the 7 upstream skills into one skill with a single description.
  - `SKILL.md` is new: one description and an index of topic files.
  - Each upstream skill's `SKILL.md` is now a topic file with its
    frontmatter removed, renamed as follows:
    `analyzing-dotnet-performance` → `performance-analysis.md`,
    `binlog-failure-analysis` → `msbuild-binlogs.md`,
    `csharp-scripts` → `csharp-scripts.md`,
    `dotnet-webapi` → `webapi.md`,
    `microbenchmarking` → `benchmarkdotnet.md`,
    `optimizing-ef-core-queries` → `ef-core-queries.md`,
    `vectorization` → `simd.md`.
  - Each upstream `references/*` is now `references/<topic>/*`, with the
    paths in the topic files updated.
  - Mentions of `optimizing-ef-core-queries` rewritten to links to
    `ef-core-queries.md`; "this skill" wording changed to "this topic" where
    it meant the single topic.
  - Otherwise body text and support files verbatim.
