# Provenance

## grilling

- **Upstream**: mattpocock/skills — https://github.com/mattpocock/skills —
  `skills/productivity/grilling/` (fetched at commit
  `59db609eb1e18aaf59b63284d0e52255aff0c63c`, 2026-10-27)
- **License**: MIT (repo-wide LICENSE, Copyright (c) 2026 Matt Pocock),
  copied as `LICENSE.txt`. No separate per-skill license file upstream.
- **Adapted from the original**:
  - `description:` rewritten to be trigger-rich — upstream's relied on
    "any 'grill' trigger phrases" without naming them.
  - Fact-finding paragraph: "dispatch a sub-agent to find it" softened to
    "look it up yourself — dispatch a sub-agent when one is available",
    since not every CLI/agent manigot runs can spawn sub-agents; "code" and
    "docs" added to the example fact sources.
  - Added a closing paragraph for headless runs (no human to answer):
    list the open decisions with recommendations and stop.
  - Dropped upstream's `agents/openai.yaml` (vendor-specific UI metadata).
  - Otherwise verbatim.
