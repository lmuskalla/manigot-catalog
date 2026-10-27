---
name: go-guidelines
description: Use for any task that writes, edits, refactors, tests, or reviews Go code — idiomatic Go style from Effective Go, the Google and Uber style guides, and Go Code Review Comments. Covers naming, error handling (%w, errors.Is/As, sentinels), interfaces, generics, goroutines/channels/mutexes, context.Context, slices and maps, functional options, slog logging, table-driven tests, benchmarks, godoc, packages and imports, and golangci-lint setup. Apply while writing code, not only in review.
---

# Go Guidelines

One entry point for Go. This file holds the rules that apply to every Go change
and an index of topic files. Open only the topics the task touches; each topic
file routes further to its own `references/<topic>/` files, scripts, and assets.

All paths in this skill (topic files, `references/`, `scripts/`, `assets/`) are
relative to this skill directory.

## Always

1. Run `gofmt` (or `goimports`) — no exceptions. Then `go vet ./...`.
2. Priority order when rules conflict: clarity > simplicity > concision >
   maintainability > consistency with the surrounding code.
3. Handle every error once: either return it (wrapped with context) or log
   it — never both, never silently drop it.
4. MixedCaps names, no `Get` prefixes, no stutter (`http.Server`, not
   `http.HTTPServer`), short receivers, no `this`/`self`.
5. Accept interfaces, return concrete types; define interfaces at the consumer.
6. `ctx context.Context` is the first parameter; never store it in a struct.
7. Every goroutine has a clear owner and a way to stop.
8. Prefer early returns over nesting; keep the happy path left-aligned.
9. Exported identifiers get doc comments starting with the identifier's name.

## Topics

| Topic | Open when |
|-------|-----------|
| [style-core.md](style-core.md) | Formatting, line length, nesting, naked returns, or a style question no other topic covers |
| [naming.md](naming.md) | Naming packages, types, functions, methods, variables, constants, or receivers |
| [declarations.md](declarations.md) | `var` vs `:=`, scope, struct/map/composite literals, `iota` enums, shadowing |
| [control-flow.md](control-flow.md) | `if` with init, guard clauses, `for`/`range` forms, `switch`, blank identifier |
| [functions.md](functions.md) | Function order within a file, signature formatting, return values, Printf/Stringer |
| [functional-options.md](functional-options.md) | A `New*` constructor with several optional settings |
| [data-structures.md](data-structures.md) | Slices, maps, arrays, `new` vs `make`, `append`, nil vs empty, sets |
| [interfaces.md](interfaces.md) | Defining or implementing interfaces, embedding, type assertions/switches, mockable boundaries |
| [generics.md](generics.md) | Whether to use type parameters, constraints, type aliases vs definitions |
| [error-handling.md](error-handling.md) | Returning, wrapping (`%w` vs `%v`), sentinel/custom errors, `errors.Is`/`As`, log-or-return |
| [defensive.md](defensive.md) | API-boundary copies, `defer` cleanup, `time.Time`/`Duration`, globals, panic/recover, `Must*` |
| [concurrency.md](concurrency.md) | Goroutines, channels, mutexes, `sync`, data races, worker pools, `errgroup` |
| [context.md](context.md) | `context.Context` placement, cancellation, deadlines, request-scoped values |
| [logging.md](logging.md) | `log/slog`, structured fields, log levels, request-scoped loggers |
| [packages.md](packages.md) | Package boundaries and size, import grouping, dependencies, `init()` |
| [documentation.md](documentation.md) | Doc comments for packages and exported APIs, runnable examples |
| [testing.md](testing.md) | Writing tests: table-driven, subtests, `t.Parallel`, helpers, fakes, `cmp.Diff` |
| [performance.md](performance.md) | Slow code, allocations, string building, `strconv` vs `fmt`, benchmarks |
| [linting.md](linting.md) | Setting up golangci-lint or adding Go checks to CI |
| [code-review.md](code-review.md) | Reviewing Go changes or preparing a Go PR — the full checklist |

Typical combinations: new code → the topics for what you are writing plus
`naming.md` and `error-handling.md`; tests → `testing.md`; review →
`code-review.md`, which links back to the others.

## Checks

| Script | Purpose |
|--------|---------|
| `scripts/pre-review.sh` | gofmt, go vet, and golangci-lint in one pass before a review |
| `scripts/check-errors.sh` | String-matched errors, bare propagation, log-and-return |
| `scripts/check-naming.sh` | Naming anti-patterns |
| `scripts/check-docs.sh` | Missing doc comments on exported symbols |
| `scripts/check-interface-compliance.sh` | Exported interfaces lacking compile-time assertions |
| `scripts/bench-compare.sh` | Compare benchmark runs against a baseline |
| `scripts/setup-lint.sh` | Generate a `.golangci.yml` baseline |
| `scripts/gen-table-test.sh` | Scaffold a table-driven test |
