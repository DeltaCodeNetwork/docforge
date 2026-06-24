# Research summary — docs for coding agents

> **TL;DR.** For a wiki whose *primary* reader is a coding agent (and humans second), the evidence favors a **layered hybrid** — lean agent index + human Diátaxis wiki over an immutable source of truth — not any single framework. The dominant practical finding: context is finite with diminishing returns, so the win is the *smallest high-signal token set* loaded *just-in-time*; bloated context files measurably hurt. This file records the *why* behind [`standard.md`](./standard.md). It is a snapshot (mid-2026) of a fast-moving area; treat preprints and vendor self-reports with appropriate skepticism.

## What the evidence says

1. **Context rot is real and gradual.** LLM precision drops as input grows — across 18 frontier models, even on trivial tasks; it's a gradient, not a cliff. The goal is "the smallest possible set of high-signal tokens." → motivates token economy and just-in-time loading.
   - Chroma, *Context Rot* — https://www.trychroma.com/research/context-rot
   - Anthropic, *Effective context engineering for AI agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
   - (Corroborated by Liu et al., *Lost in the Middle*.)

2. **Position matters as much as presence.** Models scored *higher* on shuffled haystacks than logically-ordered ones with identical content — *how* information is arranged beats merely having it present.
   - Chroma (above).

3. **Bloated context files harm coding agents.** "Unnecessary requirements from context files make tasks harder, and human-written context files should describe only minimal requirements"; context files raise inference cost by >20% on average (range ~13–50%).
   - Gloaguen et al. (ETH Zurich SRI Lab), *Evaluating AGENTS.md*, Feb 2026 — https://arxiv.org/abs/2602.11988

4. **Value comes only from non-redundant, tacit knowledge.** Developer-written context files improved success ~4% precisely because they captured conventions "in developers' heads but not in any documentation." LLM-generated files that restate existing docs/code did not help.
   - Upsun, *AGENTS.md: less is more* — https://www.upsun.com/blog/agents-md-less-is-more/
   - Gloaguen et al. (above) — confirmed via a doc-removal experiment.

5. **Just-in-time / progressive disclosure beats front-loading.** Agents should keep lightweight identifiers (paths, queries, links) and load detail at runtime; folder hierarchies and naming conventions are themselves navigation signal. Anthropic endorses a *hybrid* (a small amount retrieved up front for speed) — i.e. the lean index.
   - Anthropic (above).

## The approaches, weighed

- **Diátaxis** (https://diataxis.fr) — organizes docs by four user *needs* (tutorial / how-to / reference / explanation). Excellent for *human* comprehension; **no surveyed source links it to agent-task success**. Verdict: keep it as the human-layer organizer, not as something the agent is told about. (Human-side criticism of rigidity: Hillel Wayne, *Problems with the 4doc model* — https://www.hillelwayne.com/post/problems-with-the-4doc-model/.)
- **Karpathy's three-layer knowledge architecture** (https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — immutable raw sources → an LLM-maintained Markdown wiki of atomic, cross-linked pages → a schema/index file telling the agent how it's structured and how to maintain it. Maps cleanly onto the evidence; names contradictions/stale-claims/orphans as lint targets. (Caveat: describes a personal knowledge base; generalized here to engineering docs.)
- **llms.txt / AGENTS.md / CLAUDE.md** — emerging conventions for a curated, flattened agent-facing index (llms.txt: https://llmstxt.org). These *are* the lean-index layer; they work when minimal. AGENTS.md is consolidating as a cross-tool standard.
- **Graph / GraphRAG (e.g. graphify, Codebase-Memory)** — convert a project into a queryable knowledge graph served over MCP; prefer scoped queries over grep. Independent-ish benchmark: ~83% answer quality vs ~92% for file exploration, at ~10× fewer tokens and ~2.1× fewer tool calls; wins on *structural* queries, loses when full source context is needed. Verdict: a scale complement, not a replacement.
  - graphify — https://github.com/safishamsi/graphify
  - Vogel et al., *Codebase-Memory*, arXiv 2603.27277v1 — https://arxiv.org/html/2603.27277v1

## The recommendation (→ the standard)

Keep docs human-readable and Diátaxis-organized (L2), layer a minimal agent index + memory on top (L3) over an immutable source of truth (L1), enforce single-source-of-truth / no-orphans / a staleness lint, and prefer just-in-time loading over front-loading. Add a graph/MCP index only when codebase scale makes grep expensive.

## Caveats & open questions

- **No controlled benchmark compares documentation *taxonomies* (Diátaxis vs flat vs three-layer vs llms.txt-indexed) on identical coding-agent task suites.** The "Diátaxis-as-human-layer" position rests on the *absence* of agent-benefit evidence plus its established human value — not a head-to-head test.
- Several key sources are recent **preprints or vendor/author self-reports** (the two arXiv papers; Chroma is a vector-DB vendor; graphify's "71.5× fewer tokens" is self-reported). The context-rot finding is the most robust; the graph numbers are the softest.
- The ">20% cost" is an average across models/datasets (human-written files closer to ~19%).
- A refuted claim worth noting: AGENTS.md files do **not** outright *reduce* success vs no context — they're net-neutral-to-slightly-positive when minimal, harmful when bloated. The lesson is "keep it lean," not "skip it."
- Operationalizing the lint/staleness loop reliably (cadence, adjudication against the source of truth) is unproven in the literature.

## Related

- [`standard.md`](./standard.md) — the conventions these findings produced.
