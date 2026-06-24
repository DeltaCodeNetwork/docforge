# Writing docs/

Authoring rules for this project's wiki, from the [DocForge Standard](https://github.com/DeltaCodeNetwork/docforge/blob/main/reference/standard.md).

- Every page starts with YAML front-matter (`title, type, status, owner, created, last_updated, tags`, ISO-8601 dates) and a one-paragraph **TL;DR**.
- **Atomic:** one concept per page; stable descriptive headings (headings are addresses — never "Notes 2").
- **Single source of truth:** state each fact once and link to it; never restate what another page or a generator (OpenAPI/schema) owns, and never copy code facts the reader can get from the code.
- **Diátaxis:** don't mix types — `tutorials/` (learning), `how-to/` (a goal), `reference/` (facts), `architecture/` (explanation/why).
- End every page with a **Related** section cross-linking sibling pages + the relevant ADR(s)/runbook(s). No orphans.
- Diagrams are Mermaid (title + legend; label relationships with intent, not "uses").
- **ADRs (`docs/decisions/`) are immutable** — supersede, never edit a decided one (MADR).
- **Runbooks** carry specific commands and **never secrets** (reference the secret manager).
- Don't invent facts; cite code as `path:line`; mark unknowns `TODO`. New work lands `status: draft`. Bump `last_updated` on edit.
- **Keep the agent index (`AGENTS.md`/`CLAUDE.md`) lean** — pointers and tacit knowledge only, never inlined page content.
- Agents draft; a human reviews. Never auto-merge agent-written docs.
