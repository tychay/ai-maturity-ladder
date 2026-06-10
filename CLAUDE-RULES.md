# AI Maturity Ladder — Operational Rules

See `assets/ai-maturity-ladder.md` for the full philosophy.

## Rules

- Start by doing things directly. As patterns repeat, graduate them:
  act → skill → decomposed sub-skills → workflow (code orchestration) → tools (code execution)
- **Graduation test:** Can this step be expressed without natural language reasoning?
  If yes, it should be code. AI belongs only where:
  (1) unstructured→structured, (2) structured→unstructured, or (3) conditions require semantic judgment.
- **Code-mode principle:** Always wrap external service calls (MCP or otherwise) as typed TypeScript APIs.
  This enables tool-chaining without AI mediation.
- **Trigger axis (separate from the ladder):** Once a workflow exists, its invocation can graduate:
  manual → Claude Code hook → system event (cron/fswatch/webhook).
- Use OpenSpec/SDD when building tools at any level.
- Feel free to suggest when something is ready to graduate (see `/graduation-suggest`).
