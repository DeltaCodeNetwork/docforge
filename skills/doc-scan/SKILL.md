---
description: Backfill the wiki from the existing codebase as drafts (two-tiered, opt-in). Use to bootstrap docs/ for a project that has code but little documentation.
argument-hint: "Optional: a path or subsystem to focus the scan on"
---

Run the `doc-scanner` subagent to backfill `docs/` from the existing codebase, following the [DocForge Standard](${CLAUDE_PLUGIN_ROOT}/reference/standard.md). Everything produced is a **draft for human review** — the biggest risk here is confident, well-structured, WRONG docs.

Two tiers:
- **Tier 1 — code-derived (safe to draft, `status: draft`):** repo map, entry points, component/reference pages with *real* interfaces read from code, the dependency list, and `architecture/overview.md` with a Mermaid C4 Context + Container diagram from the actual structure. These cite code (`path:line`).
- **Tier 2 — inferred (propose-only, `status: draft` + explicit `TODO`/`needs-confirmation`):** ADR candidates mined from `git log`/`git blame`, runbook candidates from console commands / CI / ops scripts, and a coverage/gap report. Never asserted as fact.

Hard rules: Bash is read-only (`git log`, `git blame`, `find`, `grep`, `ls` only — never mutate the repo or run the app). Never invent facts; never auto-promote a draft to verified; never edit a decided ADR. Skip pages that already exist unless asked to refresh. Then suggest `/docforge:doc-audit` to review the result.
