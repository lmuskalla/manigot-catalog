# Provenance

## react-best-practices

- **Upstream**: vercel-labs/agent-skills - https://github.com/vercel-labs/agent-skills
  - `skills/...` (fetched from `main`, 2026-10-09); original to Vercel Engineering.
- **License**: MIT - declared in the skill frontmatter and repo README; the
  upstream repo ships no LICENSE file, so `LICENSE.txt` here is the canonical
  MIT text attributed to Vercel.
- **Adapted from the original**:
  - Renamed to `react-best-practices` (dropped the `vercel-` prefix).
  - Frontmatter reduced to `name:` + one-line `description:`; `license:`/
    `metadata:` keys removed.
  - `description` rewritten to a concise, trigger-first one-liner.
  - Merged in the sibling skill `react-composition-patterns` (same
    upstream repo and license): its `SKILL.md`
    body is now `composition.md` (frontmatter removed, `AGENTS.md` reference
    renamed), its compiled `AGENTS.md` is `AGENTS-composition.md`, its rules
    moved into `rules/` (its `_sections.md` as `_sections-composition.md`),
    and its `README.md`, `metadata.json`, and `rules/_template.md` (upstream
    authoring scaffolding) dropped. `description` widened to cover it and a
    "Composition Patterns" section appended to `SKILL.md`.
  - Otherwise body and support files verbatim.
