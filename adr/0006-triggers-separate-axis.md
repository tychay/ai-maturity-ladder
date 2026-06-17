# 0006 — Triggers are a separate axis from the maturity ladder

**Status:** Active
**Date:** 2026-06-10

## Context

Claude Code hooks (zero-token shell commands fired on events) were introduced after the original 4-level ladder. A decision was needed about whether hooks/triggers should be a new ladder level or represented differently, given that hooks answer "what starts the workflow" while the ladder answers "what does the work."

## Decision

Triggers are a separate orthogonal axis with its own graduation path: manual → Claude Code hook → system event (cron/fswatch/webhook). The ladder governs what does the work; the trigger axis governs what starts the work. A trigger must never contain AI reasoning — if it does, it is a condition masquerading as a trigger.

## Consequences

The ladder remains clean as a "what does the work" scale. New trigger mechanisms don't require ladder restructuring. A deterministic tool invoked manually is complete on the work axis but not the trigger axis — graduation suggestions must distinguish between the two. Any future automation must have both axes documented. Rejected alternatives: trigger as a ladder level between 1 and 2, trigger as a ladder precondition.

---

*Extracted from [[ai-maturity-ladder]] (habits file, "Discussion about Triggers") and `openspec/changes/archive/2026-06-10-distill-maturity-ladder-rules/`, originally decided ~2026-06-10.*
