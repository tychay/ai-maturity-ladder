# 0004 — link-map.json governs wikilink resolution during distillation

**Status:** Active
**Date:** 2026-06-10

## Context

The vault source document uses Obsidian wikilinks (`[[code-mode]]`, `[[RPC]]`, etc.) that are meaningless outside the vault. The distillation to standard markdown needs a deterministic, reviewable mapping rather than ad-hoc AI judgment on each link each time.

## Decision

`link-map.json` in the submodule root maps each wikilink target to one of: a URL string (becomes a markdown link), `null` (link removed, text kept), or a relative path (links to another distilled file in the submodule). The distillation process consults this map mechanically. New unresolved wikilinks are flagged for the user to decide.

## Consequences

Resolution decisions are durable across revision cycles. The map records which vault concepts have been distilled vs. dropped. When the map grows large enough that lookup-and-replace becomes mechanical, the step can be extracted into a tool. The file is committed and versioned, so distillation is reproducible. Rejected alternative: per-run ad-hoc AI judgment.

---

*Extracted from [[ai-maturity-ladder-modularization]] and `openspec/changes/extract-maturity-ladder-submodule/`, originally decided ~2026-06-10.*
