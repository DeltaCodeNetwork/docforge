---
description: Write a blameless postmortem (5-Whys + durable preventative actions) under docs/retros/. Use after an incident or a notable near-miss.
argument-hint: "Describe the incident (what happened, impact, timeline if known)"
---

Write a blameless postmortem under `docs/retros/` using the `doc-writer` subagent, following the [DocForge Standard](${CLAUDE_PLUGIN_ROOT}/reference/standard.md).

1. Copy the retro template (`docs/_templates/retro-template.md`, or `${CLAUDE_PLUGIN_ROOT}/templates/_templates/retro-template.md`) to `docs/retros/YYYY-MM-DD-<kebab-title>.md`.
2. Reconstruct the timeline, impact, and root cause from the user's account and the codebase/logs they can point at. Use **5-Whys** to reach a root cause, not a symptom.
3. Keep it **blameless** — focus on systems and gaps, never individuals.
4. The most important section is **durable preventative actions**: concrete, owned, verifiable follow-ups (a test, a guard, a runbook, an alert). Link each to where it will live.
5. If the incident reveals a wrong or missing convention, note the doc/runbook to update (and do it via `/docforge:document`).

Cite code/logs as `path:line` where possible. Don't invent causes — mark unknowns `TODO`. A human reviews — never auto-merge.
