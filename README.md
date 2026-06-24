# DocForge

A Claude Code plugin for building and maintaining a **comprehensive, layered wiki** that serves coding agents first and humans second — grounded in research on how agents actually consume context.

It ships the conventions (the [DocForge Standard](reference/standard.md)), the automation to apply them (scaffold, draft, lint), and the evidence behind them ([research summary](reference/research-summary.md)). The standard itself is plain Markdown and tool-agnostic; the plugin is the Claude Code delivery vehicle.

## The model

Four layers (full detail in [`reference/standard.md`](reference/standard.md)):

- **L1 · Source of truth** — code, schemas, specs. Never duplicated into docs.
- **L2 · Human wiki** — atomic, cross-linked Markdown in `docs/`, organized by Diátaxis.
- **L3 · Agent index** — a lean `AGENTS.md`/`CLAUDE.md` + memory that *points* into L2/L1 and carries only non-obvious, tacit knowledge.
- **L0 · This standard** — written once, instantiated into every project.

The guiding rule, from the evidence: give the agent the **smallest set of high-signal tokens** and let it load detail **just-in-time** — bloated context files measurably hurt.

## Install

```bash
/plugin marketplace add DeltaCodeNetwork/docforge
/plugin install docforge@docforge
```

Install globally (available in every project) or per-project (`--scope project`, shared with your team via git).

## Skills

| Command | What it does |
|---|---|
| `/docforge:init` | Scaffold the layered structure into a project (docs skeleton + lean index + rules + templates) |
| `/docforge:document` | Draft or update a wiki page in the right Diátaxis genre, from the right template |
| `/docforge:adr` | Record an immutable architecture decision (MADR) under `docs/decisions/` |
| `/docforge:doc-audit` | Read-only lint: staleness, orphans, contradictions, broken links, a bloated index |
| `/docforge:doc-scan` | Backfill the wiki from the existing codebase as drafts (two-tiered, opt-in) |
| `/docforge:retro` | Write a blameless postmortem under `docs/retros/` |

Agents drafting these never auto-merge — a human reviews. ADRs are immutable (supersede, never edit).

## Adopting it in a project

1. Install the plugin (above).
2. Run `/docforge:init` at the repo root — it scaffolds `docs/`, a lean `AGENTS.md`, the page templates, and the authoring rules, and links back to this standard.
3. `/docforge:doc-scan` to backfill drafts from existing code, then review.
4. Wire `/docforge:doc-audit` into review/CI as the recurring lint pass.

## Layout

```
docforge/
├── .claude-plugin/        # marketplace.json + plugin.json
├── reference/             # standard.md (the conventions) + research-summary.md (the why)
├── skills/                # init, document, adr, doc-audit, doc-scan, retro
├── agents/                # doc-writer, doc-auditor, doc-scanner (bundled, self-contained)
└── templates/             # docs-skeleton/, index/, rules/, _templates/
```

## Status

`v0.1.0` — initial. The graph/MCP index (for large codebases) and a CI lint hook are intentionally **future** add-ons, kept out of v0.1 to stay lean.
