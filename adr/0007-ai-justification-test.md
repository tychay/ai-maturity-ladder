# 0007 — AI Justification Test: three cases where AI is irreplaceable

**Status:** Active
**Date:** 2026-06-10

## Context

The maturity ladder requires deciding when a workflow step must remain AI-mediated vs. when it should be graduated to deterministic code. Without an explicit test, the boundary is subjective and graduation priority is hard to assess.

## Decision

AI is justified in a workflow step only for three cases: (1) Structuring — unstructured input to structured output; (2) Composing — structured input to unstructured output; (3) Judging — a condition requiring semantic understanding. Everything else (structured-to-structured transforms, routing, CRUD, data fetching) is a code smell indicating the step hasn't been graduated yet.

## Consequences

Any step that doesn't pass the test is a graduation candidate, making prioritization objective. The test defines what remains in the "reasoning kernel" after full graduation. The three cases are treated as exhaustive — if a new case appears, this ADR should be revisited. Rejected alternatives: token-cost-only heuristic, "does it feel like AI work" judgment.

---

*Extracted from [[ai-maturity-ladder]] (habits file, "AI Justification Test"), originally decided ~2026-06-10.*
