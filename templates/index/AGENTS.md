# {{PROJECT_NAME}}

> One-line purpose: {{WHAT_THIS_PROJECT_IS}}

This is the **lean agent index** (DocForge L3). It is a map, not a manual: it states only what isn't obvious from the code, and points into `docs/` for detail. Keep it short — load detail just-in-time by following the links.

## Commands

```bash
{{BUILD_COMMAND}}      # build
{{TEST_COMMAND}}       # test
{{LINT_COMMAND}}       # lint / format
```

## Conventions that aren't obvious from the code

- {{TACIT_CONVENTION_1}}
- {{TACIT_CONVENTION_2}}
<!-- Only non-redundant, tacit knowledge here — workflow/tooling rules, gotchas, "why we do it this way". If it's discoverable by reading the code, it does NOT belong here. -->

## Where things live

- Architecture & data flow → `docs/architecture/`
- How-to guides → `docs/how-to/` · Reference (per component) → `docs/reference/`
- Decisions (immutable ADRs) → `docs/decisions/` · Runbooks → `docs/runbooks/`
- Wiki hub / index → `docs/README.md`

## Working rules

- Documentation is part of Done — update `docs/` in the same change as the code.
- Significant decision → `/docforge:adr`. Operational fix → `/docforge:document` a runbook. Incident → `/docforge:retro`.
- Single source of truth: never restate here what a `docs/` page or the code already owns.

<!-- For Claude Code, CLAUDE.md may simply contain: `See @AGENTS.md` — keep one canonical copy. -->
