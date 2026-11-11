# Provenance

## systematic-debugging

- **Upstream**: obra/superpowers — https://github.com/obra/superpowers —
  `skills/systematic-debugging/` (fetched at commit
  `b36e0829c6d0140e93cfef2ca599b1b07d4a7797`, 2026-08-27)
- **License**: MIT (repo-wide LICENSE, Copyright (c) 2025 Jesse Vincent). No
  separate per-skill license file upstream.
- **Adapted from the original**:
  - `SKILL.md`: de-referenced two sibling-skill pointers that manigot does not
    ship — `superpowers:test-driven-development` → `test-driven-development`
    (shipped) and `superpowers:verification-before-completion` → a plain
    "verify the fix before claiming success" instruction.
  - `root-cause-tracing.md`: changed the `find-polluter.sh` invocation from
    `./find-polluter.sh` to `bash find-polluter.sh`, because staged skill
    copies are written with 0644 (the +x bit is stripped), so a direct
    `./` invocation would fail in the container; `bash <script>` does not
    need the executable bit.
  - Kept verbatim (byte-for-byte identical to the upstream commit): the
    remaining in-directory support files — `defense-in-depth.md`,
    `condition-based-waiting.md`, `condition-based-waiting-example.ts`, and
    `find-polluter.sh`. Note that `find-polluter.sh`'s `${TEST_PATTERN#./}`
    strip and the dual `-path` with the `**/`-collapsed fallback (lines
    21-27, including the developer-style commentary) are present in the
    upstream file at the pinned commit and are reproduced unchanged — they
    are upstream-verbatim, not manigot modifications.
  - Dropped upstream's internal development artifacts (`CREATION-LOG.md`,
    `test-academic.md`, `test-pressure-*.md`) — not referenced by the skill
    body and not part of the skill's runtime.
  - No CLI-specific syntax anywhere; TypeScript examples are illustrative.
## Merged from mattpocock/skills `diagnosing-bugs`

- **Upstream**: mattpocock/skills — https://github.com/mattpocock/skills —
  `skills/engineering/diagnosing-bugs/` (fetched at commit
  `59db609eb1e18aaf59b63284d0e52255aff0c63c`, 2026-10-27)
- **License**: MIT (repo-wide LICENSE, Copyright (c) 2026 Matt Pocock),
  appended to `LICENSE.txt` below the obra/superpowers notice.
- **What was merged** (the four-phase structure above stays as the base):
  - `feedback-loop.md` (new): upstream's "Phase 1: Build a feedback loop"
    (ways to construct one, tighten, non-deterministic bugs, can't-build-a-loop
    escape, completion criterion) plus "Minimise" from its Phase 2, mostly
    verbatim. Adapted: "This is the skill" → "This is the core of debugging";
    the HITL script is invoked with `bash <copy>` (staged copies lose +x);
    added a headless-run line to the can't-build-a-loop escape.
  - `SKILL.md`: added the "Redact secrets" paragraph; Phase 1 step 2
    "Reproduce Consistently" replaced by a summary of the feedback-loop
    requirement pointing to `feedback-loop.md`; Phase 3 "Form Single
    Hypothesis" replaced by upstream's 3–5 ranked falsifiable hypotheses
    shown to the user; Phase 3 "Test Minimally" gained upstream's Phase 4
    instrumentation rules (debugger first, tagged `[DEBUG-xxxx]` logs, perf
    baseline-then-bisect); Phase 4 gained the correct-seam rule for the
    regression test, the re-run-the-original-loop check, and the cleanup
    checklist; one red flag added; quick-reference table, supporting-techniques
    list, and `description:` updated to match (description also gained
    performance/flaky/slow and "debug this"/"diagnose" triggers).
  - `hitl-loop.template.sh`: verbatim copy of upstream
    `scripts/hitl-loop.template.sh`, placed at the skill root beside
    `find-polluter.sh`.
  - Not merged: the instruction to read `GLOSSARY.md`/ADRs (a convention of
    upstream's own repo setup), and upstream's `agents/openai.yaml`.
