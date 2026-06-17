# 0002 — Submodule structure: CLAUDE-RULES.md (operational) + assets/ (philosophy) + README.md (humans)

**Status:** Active
**Date:** 2026-06-10

## Context

When packaging the maturity ladder as a submodule, content needed to be split between what Claude reads operationally at session start, what Claude reads on demand for ambiguous cases, and what humans read for setup. A single file forces Claude to always read too much or too little.

## Decision

Split into three concerns: README.md (human-facing: what it is, install/use instructions), CLAUDE-RULES.md (always-loaded operational rules for Claude), and `assets/ai-maturity-ladder.md` (philosophy document, read on demand). CLAUDE.md in host projects references CLAUDE-RULES.md directly; assets/ is consulted when deeper context is needed.

## Consequences

Future distilled files (e.g., `assets/code-mode.md`) slot into `assets/` without cluttering the top level. CLAUDE.md stays minimal — one reference line per file. Claude can act without re-reading philosophy each session but consults `assets/` for ambiguous cases. README.md is never in Claude's operational context. Rejected alternative: single README.md for everything.

---

*Extracted from [[ai-maturity-ladder-modularization]] and `openspec/changes/archive/2026-06-10-distill-maturity-ladder-rules/`, originally decided ~2026-06-10.*
