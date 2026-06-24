---
description: Draft or update a wiki page in docs/ — pick the right Diátaxis genre, template, and folder. Use to document a feature, component, how-to, or architecture.
argument-hint: "What you want to document (a feature, component, how-to, runbook, ...)"
---

Document something in `docs/` using the `doc-writer` subagent, following the [DocForge Standard](${CLAUDE_PLUGIN_ROOT}/reference/standard.md).

1. Choose the genre + Diátaxis folder by the *need* it serves:
   - learning by doing → `docs/tutorials/` (tutorial-template)
   - accomplish a goal → `docs/how-to/` (how-to-template)
   - facts looked up while working → `docs/reference/` (reference-template; one page per component/service)
   - understand how it fits + why → `docs/architecture/` (explanation-template)
   - an operational procedure → `docs/runbooks/` (runbook-template)
   - an architecture decision → use `/docforge:adr` instead
   - an incident → use `/docforge:retro` instead
2. Start from the matching template in `docs/_templates/` (or `${CLAUDE_PLUGIN_ROOT}/templates/_templates/` if the project hasn't scaffolded yet). Keep front-matter + TL;DR + atomic scope + a `Related` section.
3. State each fact once and link to its single source of truth. Cite code as `path:line`. Embed a Mermaid diagram if it clarifies.
4. New pages are `status: draft`; cross-link from `docs/README.md` and sibling pages, and update the lean index (`AGENTS.md`) **only** if a new top-level area was added — never inline the page's content there.

Update docs in the same change as the code they describe. A human reviews the draft — never auto-merge.
