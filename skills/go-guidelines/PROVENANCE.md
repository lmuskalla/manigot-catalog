# Provenance

## go-guidelines

- **Upstream**: cxuu/golang-skills - https://github.com/cxuu/golang-skills
  - `skills/go-*/` — all 20 skills (fetched from `main`, 2026-10-09).
- **License**: Apache-2.0 (see `LICENSE.txt`).
- **Adapted from the original**:
  - Merged the 20 upstream skills into one skill. Upstream's per-topic
    descriptions overlapped and nearly all triggered on any Go code, so a
    single entry point with on-demand topic files replaces them.
  - `SKILL.md` is new: one description, cross-cutting rules distilled from
    the upstream topics, and an index of topic files and scripts.
  - Each upstream `skills/go-<topic>/SKILL.md` is now `<topic>.md` with its
    frontmatter removed (upstream descriptions condensed into the index).
  - Each upstream `skills/go-<topic>/references/*` is now
    `references/<topic>/*`; `scripts/` and `assets/` are merged flat (no name
    collisions).
  - Cross-skill links (`../go-<topic>/SKILL.md`) and mentions (`go-<topic>`)
    rewritten to the topic files; "skill" wording that referred to a sibling
    skill changed to "topic", and "Related Skills" headings to "Related
    Topics".
  - Otherwise body text, references, scripts, and assets verbatim.
