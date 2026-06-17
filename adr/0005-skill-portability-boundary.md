# 0005 — graduation-suggest in submodule; revise-maturity-ladder in parent project

**Status:** Active
**Date:** 2026-06-10

## Context

The submodule contains two distinct behaviors: proactively suggesting graduation opportunities (portable across all projects) and revising the ladder itself (requires vault access and a research-discuss file that don't exist in other projects). Putting both in the submodule would make one skill non-functional in projects that lack the vault.

## Decision

`graduation-suggest` lives in the submodule under `skills/` and is auto-discovered by Claude Code in any project that includes the submodule. `revise-maturity-ladder` lives in the parent project's `.claude/skills/` and is intentionally not portable.

## Consequences

Projects that include the submodule get graduation suggestions for free. They do not get the revision loop — they would need to implement their own if they want to contribute philosophy changes. The boundary makes the portability contract explicit: portable behaviors in submodule `skills/`, project-specific behaviors in `.claude/skills/`. Rejected alternative: both in submodule.

---

*Extracted from `openspec/changes/extract-maturity-ladder-submodule/`, originally decided ~2026-06-10.*
