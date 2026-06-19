# 0011 — Claude Code plugin distribution over ai/ namespace convention

**Status:** Active
**Supersedes:** 0001
**Date:** 2026-06-18

## Context

ADR-0001 established `ai/` as a custom namespace for methodology submodules. Since then, Claude Code shipped a formal plugin system (Oct 2025) that solves the same distribution problem — marketplace discovery, namespaced skills, setup lifecycle, version management. The `ai/` convention was a bespoke solution to a problem the ecosystem already solved. OpenCode is also converging on `.claude/skills/` as a fallback discovery path.

## Decision

Distribute the maturity ladder as a Claude Code plugin. It installs to `.claude/skills/aiml/` (project) or `~/.claude/skills/aiml/` (global) via `claude plugin install`. The `ai/` namespace directory is sunset. For development repos that track `.claude/` in version control, use a git submodule at `.claude/skills/aiml/` which auto-loads as `aiml@skills-dir`.

## Consequences

Skills are namespaced as `/aiml:*` (graduation-suggest, setup). The setup skill handles instruction-file wiring (replacing manual CLAUDE.md editing). Raw-submodule usage at arbitrary paths is still possible but undocumented as primary. The `ai/openspec-adr` submodule will follow the same pattern in a separate change. Projects consuming this remove `ai/maturity-ladder` and run `claude plugin install` instead.

---

*Decided during convert-maturity-ladder-to-plugin change, 2026-06-18.*
