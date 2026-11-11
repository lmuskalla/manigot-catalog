---
name: dotnet-guidelines
description: Use for .NET and C# tasks — ASP.NET Core Web API endpoints (controllers, minimal APIs, OpenAPI, error handling), slow Entity Framework Core queries, scanning C# for performance anti-patterns, BenchmarkDotNet microbenchmarks, SIMD vectorization (Vector128/256, TensorPrimitives, intrinsics), diagnosing MSBuild failures from .binlog files, and running file-based C# apps with `dotnet run app.cs`.
---

# .NET Guidelines

One entry point for .NET. This file indexes the topic files; open only the
topics the task touches. Each topic has its own "When to Use" / "When Not to
Use" section — check it before applying the topic.

All paths in this skill (topic files, `references/`) are relative to this
skill directory.

## Topics

| Topic | Open when |
|-------|-----------|
| [webapi.md](webapi.md) | Adding or changing ASP.NET Core Web API endpoints, OpenAPI/Swagger, `.http` files, global error handling |
| [ef-core-queries.md](ef-core-queries.md) | An EF Core / `DbContext` query or data-access path is slow: N+1, too much SQL, cartesian `Include`s, missing indexes |
| [performance-analysis.md](performance-analysis.md) | Scanning C# for performance anti-patterns: async, allocations, strings, collections, LINQ, regex, serialization, I/O |
| [benchmarkdotnet.md](benchmarkdotnet.md) | Writing, running, configuring, or reviewing BenchmarkDotNet benchmarks, or measuring a .NET performance question |
| [simd.md](simd.md) | Vectorizing loops or reviewing SIMD code: `Vector64`–`Vector512`, `TensorPrimitives`, hardware intrinsics |
| [msbuild-binlogs.md](msbuild-binlogs.md) | Diagnosing MSBuild build failures from an existing `.binlog` |
| [csharp-scripts.md](csharp-scripts.md) | The user wants a quick C# experiment or file-based app without creating a project |

Typical combinations: a slow endpoint → `performance-analysis.md` or
`ef-core-queries.md`, then `benchmarkdotnet.md` to measure the fix; SIMD work →
`simd.md` plus `benchmarkdotnet.md`.
