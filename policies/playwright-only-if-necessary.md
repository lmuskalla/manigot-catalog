# Browser automation only if necessary

Prefer static analysis, reading the code, or a lighter test over browser
automation; reach for Playwright (or a `shot` render) only when those cannot
answer the question.
Driving a browser is slow and costly, so it should be the exception, not the
default.
