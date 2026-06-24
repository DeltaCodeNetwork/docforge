# The DocForge Standard

> **TL;DR.** A project's documentation should be built in four layers — an immutable **source of truth** (code/specs), a human-readable **wiki** organized by Diátaxis, a **lean agent index** that points into the wiki rather than inlining it, and this shared **standard** on top. Optimize the agent-facing layers for the *smallest set of high-signal tokens* and *just-in-time* loading; keep the human layer need-based and atomic. The evidence behind every rule here is in [`research-summary.md`](./research-summary.md).

This is the version-controlled, tool-agnostic standard that DocForge instantiates into a project. It is plain Markdown so it works for any coding agent (Claude Code, Cursor, Copilot, …) and any human. The `docforge` plugin adds the automation (scaffold, draft, lint) on top — but a project can follow this standard with no tooling at all.

---

## The four layers

| Layer | What it is | Who reads it | Optimize for |
|---|---|---|---|
| **L1 · Source of truth** | Code, schemas, specs, generated API docs | Both | Authoritative. Never duplicated into the wiki. |
| **L2 · Human wiki** | Atomic, cross-linked Markdown in `docs/`, organized by Diátaxis | Humans (primary), agents (secondary) | Comprehension, single source of truth, survives staleness |
| **L3 · Agent index** | A lean `AGENTS.md`/`CLAUDE.md` at the repo root + a memory store | Agents (primary) | Smallest high-signal token set; *points*, never inlines; just-in-time loading |
| **L0 · This standard** | The conventions + plugin, shared across all projects | Maintainers + agents | Write once, instantiate per project |

The layers are a gradient of *volatility and audience*: L1 is the truth and changes with every commit; L2 explains and changes with features; L3 is a thin, stable map an agent reads first; L0 barely changes.

---

## Principles (each maps to evidence)

### 1. The agent index is a lean map, not a manual
The strongest empirical finding for coding agents is that **bloated context files actively hurt** — they raise inference cost by >20% and slightly lower task success. The agent index (L3) must hold the *smallest set of high-signal tokens*: project identity, the few non-obvious conventions, and **pointers** (paths, links, commands) into L2/L1. It must never restate what the code already says. If a fact is discoverable by reading the code, it does not belong in the index.

### 2. Progressive disclosure / just-in-time over front-loading
Context is a finite resource with diminishing returns — precision degrades as the window fills ("context rot"). So the index gives the agent *lightweight identifiers* and lets it load detail on demand, rather than front-loading everything. Descriptive folder names and a stable hierarchy are themselves navigation signal — lean on them.

### 3. The agent layer carries only non-redundant, tacit knowledge
Context files help *only* when they capture what isn't already in the repo: tooling preferences, workflow conventions, gotchas, "why we do it this way" — the things that live in a developer's head. A restatement of the README or the code is dead weight. Put tacit knowledge in the index/memory; put everything else in the wiki, stated once.

### 4. Single source of truth, always
State each fact exactly once and link to it. Never restate what another page — or a generator (OpenAPI/JSON-Schema) — owns. Duplication is how wikis start to contradict themselves; contradiction is the failure mode that silently poisons both human and agent trust.

### 5. Atomic, cross-linked pages
One concept per page. Stable, descriptive headings (headings are addresses — never rename to "Notes 2"). Every page ends with a **Related** section linking siblings and the relevant decisions/runbooks. Orphan pages (no inbound links) are invisible to navigation and are treated as a defect.

### 6. Diátaxis organizes the human layer — and only the human layer
The wiki is organized by *need*, not by feature, into the four Diátaxis modes plus two operational genres:

- `tutorials/` — learning by doing
- `how-to/` — accomplishing a specific goal
- `reference/` — facts looked up while working (one page per component/service)
- `architecture/` — explanation: how it fits together and *why*
- `decisions/` — immutable ADRs (MADR): one decision per file
- `runbooks/` — operational procedures (specific commands; never secrets)

Diátaxis is excellent for human comprehension. There is **no evidence it improves agent task success** — so it earns its place as the human-facing structure, while the agent reaches it through the lean index, not by being told "we use Diátaxis."

### 7. Staleness is a first-class failure mode with a mechanism
A page can pass every structural check and still be *wrong*. Defend against it with:
- **Front-matter on every page** (`title, type, status, owner, created, last_updated, tags`, ISO-8601 dates) so staleness is *detectable*.
- **A periodic lint pass** (`/docforge:doc-audit`) that flags stale pages, orphans, contradictions, broken links, and a bloated index — run it in review or CI.
- **`draft` → reviewed** status: agent-authored pages land as `status: draft`; a human promotes them.

### 8. Documentation is part of Done; agents draft, humans approve
Update the docs in the *same change* as the code they describe — a feature isn't done until its wiki/index impact is reflected. Agents may draft and update pages, but a human reviews before merge. **Never auto-merge agent-written docs**, and **never edit a decided ADR** — supersede it.

### 9. A graph/MCP index is an optional scale tool, not a default
A knowledge-graph index (e.g. served over MCP) can cut query tokens ~10× at roughly a ~9% answer-quality cost. It earns its place only when a codebase is large enough that grep/file-reads are expensive. It complements lean prose docs; it does not replace them. Add it late, deliberately, never by default.

---

## What goes where (the litmus test)

- **Is it the truth itself?** → L1 (code/spec). Don't copy it anywhere.
- **Does a human need to understand or look it up?** → L2 wiki, in the right Diátaxis folder, stated once.
- **Does an agent need it to navigate or follow a non-obvious convention?** → L3 index, as a pointer or a one-line rule. If it's longer than a few lines, it belongs in L2 and the index just links to it.
- **Is it tacit "why/how-we-work" knowledge not in the code?** → L3 index or memory.

---

## The instantiated shape

```
repo/
├── AGENTS.md            # L3 — lean index (CLAUDE.md may mirror/point to it)
├── docs/                # L2 — the human wiki
│   ├── README.md        #   the hub: one-screen map of the wiki
│   ├── tutorials/  how-to/  reference/  architecture/
│   ├── decisions/       #   immutable ADRs
│   ├── runbooks/
│   └── _templates/      #   page templates
└── (code, specs)        # L1 — source of truth
```

- **`AGENTS.md`** is the agent's entry point: identity, build/test commands, the handful of conventions that aren't obvious from the code, and links into `docs/`. Keep it short. For Claude Code, `CLAUDE.md` may hold the same content or simply point at `AGENTS.md` (don't maintain two copies — one is canonical).
- **`docs/README.md`** is the human entry point: a one-screen map linking to each section and the most-used pages.
- **Memory** (where the agent supports it) holds durable, cross-session tacit knowledge — the same "non-redundant" bar as the index.

---

## Authoring rules (apply to every page)

- YAML front-matter + a one-paragraph **TL;DR** first. ISO-8601 dates. Bump `last_updated` on edit.
- Atomic; stable headings; a **Related** section cross-linking siblings + decisions/runbooks.
- State each fact once; link to its single source of truth; never restate a generator's output.
- Diagrams are Mermaid (give them a title + legend; label edges with intent, not "uses").
- **Never invent facts.** Cite code as `path:line`. Mark unknowns `TODO`/`needs-confirmation`.
- **No ticket/issue references in code comments or docblocks** — traceability lives in commits/branches; descriptions stand on their own.
- ADRs are immutable (MADR): supersede, never edit a decided one.

---

## Related

- [`research-summary.md`](./research-summary.md) — the cited evidence behind these rules
- Templates: [`../templates/_templates/`](../templates/_templates/) · Skeleton: [`../templates/docs-skeleton/`](../templates/docs-skeleton/) · Index template: [`../templates/index/`](../templates/index/)
- Skills: `/docforge:init` (scaffold) · `/docforge:document` · `/docforge:adr` · `/docforge:doc-audit` (lint) · `/docforge:doc-scan` (backfill) · `/docforge:retro`
