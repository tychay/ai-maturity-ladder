# 0001 — ai/ namespace for portable AI methodology submodules

**Status:** Active
**Date:** 2026-06-10

## Context

When extracting the maturity ladder into a portable submodule, a decision was needed about where it lives in a project tree. Options included top-level (`ai-maturity-ladder/`), inside `.claude/`, or a namespaced parent directory.

## Decision

Use `ai/` as a top-level namespace directory for AI methodology submodules (e.g., `ai/maturity-ladder/`, `ai/openspec-adr/`), paralleling the existing `tools/` namespace. The `.claude/` directory is reserved for local config, not portable methodology packages.

## Consequences

Any project consuming these packages includes submodules under `ai/`. The namespace is the convention point for future packages. CLAUDE.md references use relative paths (`ai/maturity-ladder/CLAUDE-RULES.md`) making CLAUDE.md itself portable across projects that share the same submodule layout. Rejected alternatives: top-level flat name, `.claude/packages/`, `ai-rules/`, `ai-prompts/`.

---

*Extracted from [[ai-maturity-ladder-modularization]] and `openspec/changes/extract-maturity-ladder-submodule/`, originally decided ~2026-06-10.*
