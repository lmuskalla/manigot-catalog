# Go Code Review Checklist

> Compatibility: `references/code-review/WEB-SERVER.md` uses `log/slog` examples that require Go 1.21+.

## Resource Routing

- `assets/review-template.md` - Use when formatting review output with Must Fix, Should Fix, and Nits sections.
- `scripts/pre-review.sh` - Run before manual review to collect gofmt, go vet, and golangci-lint results.
- `references/code-review/WEB-SERVER.md` - Read when reviewing an HTTP server that combines concurrency, context, logging, error handling, and shutdown behavior.

## Review Procedure

> Use `assets/review-template.md` when formatting the output of a code review to ensure consistent structure with Must Fix / Should Fix / Nits severity grouping.

1. Run `gofmt -d .` and `go vet ./...` to catch mechanical issues first
2. Read the diff file-by-file; for each file, check the categories below in order
3. Flag issues with specific line references and the rule name
4. After reviewing all files, re-read flagged items to verify they're genuine issues
5. Summarize findings grouped by severity (must-fix, should-fix, nit)

> **Validation**: After completing the review, re-read the diff once more to verify every flagged issue is real. Remove any finding you cannot justify with a specific line reference.

---

## Formatting

- [ ] **gofmt**: Code is formatted with `gofmt` or `goimports` → [linting](linting.md)

---

## Documentation

- [ ] **Comment sentences**: Comments are full sentences starting with the name being described, ending with a period → [documentation](documentation.md)
- [ ] **Doc comments**: All exported names have doc comments; non-trivial unexported declarations too → [documentation](documentation.md)
- [ ] **Package comments**: Package comment appears adjacent to package clause with no blank line → [documentation](documentation.md)
- [ ] **Named result parameters**: Only used when they clarify meaning (e.g., multiple same-type returns), not just to enable naked returns → [documentation](documentation.md)

---

## Error Handling

- [ ] **Handle errors**: No discarded errors with `_`; handle, return, or (exceptionally) panic → [error-handling](error-handling.md)
- [ ] **Error strings**: Lowercase, no punctuation (unless starting with proper noun/acronym) → [error-handling](error-handling.md)
- [ ] **In-band errors**: No magic values (-1, "", nil); use multiple returns with error or ok bool → [error-handling](error-handling.md)
- [ ] **Indent error flow**: Handle errors first and return; keep normal path at minimal indentation → [error-handling](error-handling.md)

---

## Naming

- [ ] **MixedCaps**: Use `MixedCaps` or `mixedCaps`, never underscores; unexported is `maxLength` not `MAX_LENGTH` → [naming](naming.md)
- [ ] **Initialisms**: Keep consistent case: `URL`/`url`, `ID`/`id`, `HTTP`/`http` (e.g., `ServeHTTP`, `xmlHTTPRequest`) → [naming](naming.md)
- [ ] **Variable names**: Short names for limited scope (`i`, `r`, `c`); longer names for wider scope → [naming](naming.md)
- [ ] **Receiver names**: One or two letter abbreviation of type (`c` for `Client`); no `this`, `self`, `me`; consistent across methods → [naming](naming.md)
- [ ] **Package names**: No stuttering (use `chubby.File` not `chubby.ChubbyFile`); avoid `util`, `common`, `misc` → [packages](packages.md)
- [ ] **Avoid built-in names**: Don't shadow `error`, `string`, `len`, `cap`, `append`, `copy`, `new`, `make` → [declarations](declarations.md)

---

## Concurrency

- [ ] **Goroutine lifetimes**: Clear when/whether goroutines exit; document if not obvious → [concurrency](concurrency.md)
- [ ] **Synchronous functions**: Prefer sync over async; let callers add concurrency if needed → [concurrency](concurrency.md)
- [ ] **Contexts**: First parameter; not in structs; no custom Context types; pass even if you think you don't need to → [context](context.md)

---

## Interfaces

- [ ] **Interface location**: Define in consumer package, not implementor; return concrete types from producers → [interfaces](interfaces.md)
- [ ] **No premature interfaces**: Don't define before used; don't define "for mocking" on implementor side → [interfaces](interfaces.md)
- [ ] **Receiver type**: Use pointer if mutating, has sync fields, or is large; value for small immutable types; don't mix → [interfaces](interfaces.md)

---

## Data Structures

- [ ] **Empty slices**: Prefer `var t []string` (nil) over `t := []string{}` (non-nil zero-length) → [data-structures](data-structures.md)
- [ ] **Copying**: Be careful copying structs with pointer/slice fields; don't copy `*T` methods' receivers by value → [data-structures](data-structures.md)

---

## Security

- [ ] **Crypto rand**: Use `crypto/rand` for keys, not `math/rand` → [defensive](defensive.md)
- [ ] **Don't panic**: Use error returns for normal error handling; panic only for truly exceptional cases → [defensive](defensive.md)

---

## Declarations and Initialization

- [ ] **Group similar**: Related `var`/`const`/`type` in parenthesized blocks; separate unrelated → [declarations](declarations.md)
- [ ] **var vs :=**: Use `var` for intentional zero values; `:=` for explicit assignments → [declarations](declarations.md)
- [ ] **Reduce scope**: Move declarations close to usage; use if-init to limit variable scope → [declarations](declarations.md)
- [ ] **Struct init**: Always use field names; omit zero fields; `var` for zero structs → [declarations](declarations.md)
- [ ] **Use `any`**: Prefer `any` over `interface{}` in new code → [declarations](declarations.md)

---

## Functions

- [ ] **File ordering**: Types → constructors → exported methods → unexported → utilities → [functions](functions.md)
- [ ] **Signature formatting**: All args on own lines with trailing comma when wrapping → [functions](functions.md)
- [ ] **Naked parameters**: Add `/* name */` comments for ambiguous bool/int args, or use custom types → [functions](functions.md)
- [ ] **Printf naming**: Functions accepting format strings end in `f` for `go vet` → [functions](functions.md)

---

## Style

- [ ] **Line length**: No rigid limit, but avoid uncomfortably long lines; break by semantics, not arbitrary length → [style-core](style-core.md)
- [ ] **Naked returns**: Only in short functions; explicit returns in medium/large functions → [style-core](style-core.md)
- [ ] **Pass values**: Don't use pointers just to save bytes; pass `string` not `*string` for small fixed-size types → [performance](performance.md)
- [ ] **String concatenation**: `+` for simple; `fmt.Sprintf` for formatting; `strings.Builder` for loops → [performance](performance.md)

---

## Logging

- [ ] **Use slog**: New code uses `log/slog`, not `log` or `fmt.Println` for operational logging → [logging](logging.md)
- [ ] **Structured fields**: Log messages use static strings with key-value attributes, not fmt.Sprintf → [logging](logging.md)
- [ ] **Appropriate levels**: Debug for developer tracing, Info for notable events, Warn for recoverable issues, Error for failures → [logging](logging.md)
- [ ] **No secrets in logs**: PII, credentials, and tokens are never logged → [logging](logging.md)

---

## Imports

- [ ] **Import groups**: Standard library first, then blank line, then external packages → [packages](packages.md)
- [ ] **Import renaming**: Avoid unless collision; rename local/project-specific import on collision → [packages](packages.md)
- [ ] **Import blank**: `import _ "pkg"` only in main package or tests → [packages](packages.md)
- [ ] **Import dot**: Only for circular dependency workarounds in tests → [packages](packages.md)

---

## Generics

- [ ] **When to use**: Only when multiple types share identical logic and interfaces don't suffice → [generics](generics.md)
- [ ] **Type aliases**: Use definitions for new types; aliases only for package migration → [generics](generics.md)

---

## Testing

- [ ] **Examples**: Include runnable `Example` functions or tests demonstrating usage → [documentation](documentation.md)
- [ ] **Useful test failures**: Messages include what was wrong, inputs, got, and want; order is `got != want` → [testing](testing.md)
- [ ] **TestMain**: Use only when all tests need common setup with teardown; prefer scoped helpers first → [testing](testing.md)
- [ ] **Real transports**: Prefer `httptest.NewServer` + real client over mocking HTTP → [testing](testing.md)

---

## Automated Checks

Run automated pre-review checks:

```bash
bash scripts/pre-review.sh ./...         # text output
bash scripts/pre-review.sh --json ./...  # structured JSON output
```

Or manually: `gofmt -l <path> && go vet ./... && golangci-lint run ./...`

Fix any issues before proceeding to the checklist above. For linter setup and configuration, see [linting](linting.md).

---

## Related Topics

- **Style foundations**: See [style-core](style-core.md) when resolving formatting debates or applying the clarity > simplicity > concision priority
- **Linting setup**: See [linting](linting.md) when configuring golangci-lint or adding automated checks to CI
- **Error strategy**: See [error-handling](error-handling.md) when reviewing error wrapping, sentinel errors, or the handle-once pattern
- **Naming conventions**: See [naming](naming.md) when evaluating identifier names, receiver names, or package-symbol stuttering
- **Testing patterns**: See [testing](testing.md) when reviewing test code for table-driven structure, failure messages, or helper usage
- **Concurrency safety**: See [concurrency](concurrency.md) when reviewing goroutine lifetimes, channel usage, or mutex placement
- **Logging practices**: See [logging](logging.md) when reviewing log usage, structured logging, or slog configuration
