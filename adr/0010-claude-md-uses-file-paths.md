# 0010 — CLAUDE.md references submodule by file path, not wikilink

**Status:** Active
**Date:** 2026-06-10

## Context

CLAUDE.md could reference the maturity ladder using Obsidian wikilink syntax (`[[ai-maturity-ladder]]`) or a file path. CLAUDE.md is parsed by Claude Code's harness, not rendered by Obsidian — wikilinks would not resolve to file reads.

## Decision

CLAUDE.md references the submodule using relative file paths (`ai/maturity-ladder/CLAUDE-RULES.md` and `ai/maturity-ladder/assets/ai-maturity-ladder.md`). Claude Code resolves these paths and reads the referenced files.

## Consequences

If the submodule is moved, the CLAUDE.md reference must be updated. The constraint is also the convention: any file referenced in CLAUDE.md for Claude to read must use a resolvable path, not Obsidian wikilink syntax. This applies to all CLAUDE.md files across all projects, not just this one. Rejected alternative: wikilink syntax.

---

*Extracted from `openspec/changes/archive/2026-06-10-distill-maturity-ladder-rules/`, originally decided ~2026-06-10.*
