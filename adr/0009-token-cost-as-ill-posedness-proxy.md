# 0009 — Token cost as proxy for ill-posedness, not just expense

**Status:** Active
**Date:** 2026-06-10

## Context

The rationale for graduating work down the ladder needed a principled justification beyond "tokens cost money." A mathematical analogy was developed: more tokens in context creates more degrees of freedom for the model, making the problem ill-posed (many valid solutions, no way to select the right one without more constraints).

## Decision

Token count is treated as a proxy for problem ill-posedness: fewer tokens in context = fewer degrees of freedom = fewer ways the AI can misinterpret intent = better outcomes. Graduating steps to code adds constraints (typed APIs, deterministic paths, narrowed interfaces) that reduce degrees of freedom. This framing is the canonical justification for the ladder.

## Consequences

Graduation priority is evaluated by "which steps have the highest token-to-precision-gain ratio?" not "which cost the most." The benefit of graduation is not primarily economic — a zero-cost token budget still benefits because precision improves. Future token-tracking should log tokens per step to surface graduation candidates by ill-posedness proxy. Rejected alternative: cost-only framing.

---

*Extracted from [[ai-maturity-ladder]] (habits file, "TLDR explanation") and [[ai-maturity-ladder-modularization]], originally decided ~2026-06-10.*
