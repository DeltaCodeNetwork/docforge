---
description: Record an architecture decision as a new immutable MADR ADR under docs/decisions/. Use when a significant, hard-to-reverse choice is made.
argument-hint: "Describe the decision (and the options considered)"
---

Create a new Architecture Decision Record, following the [DocForge Standard](${CLAUDE_PLUGIN_ROOT}/reference/standard.md).

1. Determine the next number `NNNN` (zero-padded, max existing in `docs/decisions/` + 1).
2. Copy the ADR template (`docs/_templates/adr-template.md`, or `${CLAUDE_PLUGIN_ROOT}/templates/_templates/adr-template.md`) to `docs/decisions/NNNN-<kebab-title>.md`.
3. Fill the front-matter (`status: proposed`, today's date, decision-makers) and the MADR sections from the user's input and the codebase. Cite code as `path:line`. Don't invent rationale — mark gaps `TODO`.
4. Cross-link: add a `Related` entry, link the ADR from the code/PR that implements it and from affected wiki pages.

Rules: **ADRs are immutable** — never edit a decided one; supersede it with a new ADR and set the old one's status to `superseded by ADR-NNNN`. Hand non-trivial drafting to the `doc-writer` subagent. A human approves — never auto-merge.
