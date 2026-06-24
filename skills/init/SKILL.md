---
description: Scaffold the DocForge layered wiki into a project — docs/ skeleton, a lean agent index, authoring rules, and page templates. Use to adopt the standard in a new or existing repo.
argument-hint: "Optional: notes about the project (stack, what it does)"
---

Instantiate the [DocForge Standard](${CLAUDE_PLUGIN_ROOT}/reference/standard.md) into the current project. Read the standard first — it defines the four layers and the rules you are about to set up.

Scaffold, idempotently (never overwrite an existing file without asking):

1. **L2 — the human wiki.** Copy `${CLAUDE_PLUGIN_ROOT}/templates/docs-skeleton/` into `docs/`, and `${CLAUDE_PLUGIN_ROOT}/templates/_templates/` into `docs/_templates/`. This creates the Diátaxis folders (`tutorials/ how-to/ reference/ architecture/`), `decisions/`, `runbooks/`, `retros/`, and the hub `docs/README.md`.
2. **L3 — the lean agent index.** Copy `${CLAUDE_PLUGIN_ROOT}/templates/index/AGENTS.md` to the repo root as `AGENTS.md`. For Claude Code, also create a `CLAUDE.md` that points at it (`See @AGENTS.md`) — one file is canonical; never maintain two copies. Fill in the project-specific placeholders by inspecting the repo: project name + one-line purpose, the real build/test/lint commands (from `composer.json`/`package.json`/`Makefile`/CI), and the handful of conventions that are NOT obvious from the code. Keep it short — it is a map, not a manual.
3. **Authoring rules.** Copy `${CLAUDE_PLUGIN_ROOT}/templates/rules/docs.md` to `.claude/rules/docs.md` (create the folder if missing).

Then:
- Tailor the hub `docs/README.md` to the project (link the sections that will actually exist).
- Report what was created and what was skipped (already present), and suggest `/docforge:doc-scan` to backfill drafts from existing code.

Rules: respect the standard — single source of truth (don't copy code facts into docs), the index stays lean and points rather than inlines, everything new lands as `status: draft` for human review. Don't invent the project's purpose or commands — read them from the repo, and mark anything you can't verify `TODO`.
