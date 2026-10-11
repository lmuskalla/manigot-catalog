---
name: report
description: Use when writing a report, audit, review, or analysis document for a project — a durable markdown deliverable that records findings and recommendations (e.g. a security audit, a design review, a performance analysis). Write it to docs/reports/ following the report structure.
---

# Writing a report

A report is a durable markdown deliverable — an audit, review, or analysis —
written for a project and kept with it. Unlike a job's `brief.md`/`tasks.md`/
`implementation.md`/`verdict.md`, which track one unit of work, a report
records a conclusion others can read later: what was examined, what was found,
and what should change.

## When to write one

Write a report when the deliverable is a document rather than a change:

- an **audit** (security, dependency, license, config) — often the output of a
  `report`-type job or a dedicated agent run;
- a **review** (design, architecture, codebase area), especially when it must
  outlive the session that produced it;
- an **analysis** (of a problem, a proposal, a piece of the codebase) that
  ends in recommendations.

The `report` job type exists for the workflow side: a `report`-type job
(`mg job "<title>" --type report`) uses the ordinary job scaffold (branch
`report/<id>_<slug>`, the usual four files) and is a fine vehicle for the
session that researches the subject — the report it produces still lands in
`docs/reports/` as a committed document.

## Where reports live

`docs/reports/<slug>.md` in the project's main worktree — one file per
report, the filename a short title-derived slug (e.g. `security-audit-2026-10.md`).
`mg init` creates an empty `docs/reports/` for every initialized project;
like `docs/backlog/` and `docs/policies/`, reports are plain committed
markdown — no branch, no id, no worktree machinery of their own.

A copy-me example lives at `project-template/docs/reports/example-report.md`
in the manigot checkout.

## Structure

Use the report template's four parts. The frontmatter is a simple non-YAML
header — `date:` always, `scope:` when it helps:

```markdown
# Report: <title>

date: YYYY-MM-DD
scope: <!-- optional — what this report covers, who it is for, or the trigger that produced it -->

## Summary

<!-- What this report is about, in a few sentences. State the conclusion up
front so a reader can decide whether to read the details. -->

## Findings

<!-- The substance of the report — one finding per section or bullet. State
what was observed, with evidence (code paths, measurements, dates) where
possible. A finding is a fact, not an opinion. -->

## Recommendations

<!-- What should change as a result of the findings? Specific and actionable —
name the change, not just the direction. -->
```

Rules of thumb:

- **Summary first, conclusion up front.** A report is read by busy people;
  the first paragraph must carry the verdict.
- **Findings are facts; recommendations are opinions.** Keep the two sections
  separate so a reader can disagree with the recommendation without rejecting
  the finding.
- **Evidence over assertion.** Cite code paths, commands, measurements, or
  dates for every finding.
- **Be specific in recommendations.** "Name the change" — a recommendation
  that cannot be actioned without further interpretation has not been written
  yet.

## Note on distribution

This checkout ships this skill so checkout-based contributors and host sessions
see it; installed binaries do not embed skills, so users of an installed `mg`
get the report skill from the defaults catalog instead (`mg defaults install
skills/report`). The structure described here is the same either way.
