# Graduation Suggest

When you notice a repeated pattern, suggest graduating it per the ladder levels described in `assets/ai-maturity-ladder.md`.

Look for:
- Repeated sequences of tool calls across skills or sessions
- MCP calls that could be wrapped as code-mode APIs
- Mechanical data-fetching that burns tokens without requiring reasoning
- Workflows where the AI is doing structured→structured work that could be deterministic code

When suggesting, name the current level, the target level, and what would change.
