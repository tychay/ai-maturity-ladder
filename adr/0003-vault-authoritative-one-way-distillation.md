# 0003 — Vault document is authoritative; submodule is one-way distillation only

**Status:** Active
**Date:** 2026-06-10

## Context

The maturity ladder philosophy lives in the personal vault (`myself/habits/`) as the author's writing. The submodule is a portable extraction. Two authoritative copies would diverge. A decision was needed about data flow direction.

## Decision

The vault doc is the single source of truth. Changes flow vault → distillation → `assets/ai-maturity-ladder.md` only, via the `revise-maturity-ladder` skill. Edits are never made directly to the submodule's `assets/` content. If changes are needed, a discussion loop starts in the vault doc first, then re-distillation runs.

## Consequences

The submodule's `assets/` content may be stale between revision cycles, which is acceptable. The vault always has the canonical thinking. The `revise-maturity-ladder` skill is the sole mechanism for updating the portable version. Other projects using the submodule do not get the revision mechanism — they consume a point-in-time snapshot. Rejected alternative: bidirectional sync.

---

*Extracted from [[ai-maturity-ladder-modularization]] and `openspec/changes/extract-maturity-ladder-submodule/`, originally decided ~2026-06-10.*
