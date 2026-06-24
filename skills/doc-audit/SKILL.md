---
description: Audit docs/ and the agent index for staleness, gaps, broken links, contradictions, orphans, and bloat (read-only report). Use as the recurring lint pass, in review or CI.
argument-hint: "Optional: a docs/ subfolder to focus on"
---

Run the read-only `doc-auditor` subagent over `docs/` (or the given subfolder) plus the agent index, and present its prioritized report. This is the **lint pass** the [DocForge Standard](${CLAUDE_PLUGIN_ROOT}/reference/standard.md) prescribes — staleness is a first-class failure mode.

It flags:
- **Stale pages** — `last_updated`/`verified` older than 90 days, or content that contradicts current code.
- **Missing coverage** — components/services with no reference doc or no `owner`.
- **Broken internal links** — relative `.md`/anchor links that don't resolve.
- **Contradictions / duplication** — the same fact stated differently across pages (single-source-of-truth violations).
- **Orphans** — pages not linked from `docs/README.md` or any `Related` section.
- **A bloated agent index** — `AGENTS.md`/`CLAUDE.md` that inlines content or restates the code/README instead of pointing into the wiki (this directly costs token budget and task success).
- **Front-matter problems** — missing required keys.

The auditor **proposes**; it never edits. A human approves fixes — never auto-merge.
