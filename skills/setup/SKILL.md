---
description: Wire the AI Maturity Ladder into a project's instruction file (CLAUDE.md, AGENTS.md, etc.)
---

# Setup

Integrate the AI Maturity Ladder rules into the current project. This skill detects the project's coding agent environment and adds the appropriate reference to the instruction file.

## Steps

1. **Detect environment** — check which instruction files exist at the project root:
   - `CLAUDE.md` → proceed with Claude Code integration
   - `AGENTS.md` (without CLAUDE.md) → report "AGENTS.md integration not yet supported" and exit
   - `.opencode/` directory → report "OpenCode integration not yet supported" and exit
   - None found → ask the user what to do (create CLAUDE.md, or abort)

2. **Check idempotency** — search the instruction file for any existing reference to `maturity-ladder/CLAUDE-RULES.md`. If found, report "AI Maturity Ladder is already configured" and exit.

3. **Resolve the path** — determine the relative path from the project root to `CLAUDE-RULES.md` in this plugin's directory. The path depends on how the plugin was installed:
   - As a project submodule: typically `.claude/skills/aiml/CLAUDE-RULES.md`
   - Other installations: use the actual path from project root to the plugin's CLAUDE-RULES.md

4. **Propose the addition** — show the user the line that will be added:
   ```
   See `<resolved-path>` for AI Maturity Ladder operational rules.
   ```
   Ask the user for confirmation before modifying the file.

5. **On approval** — append the reference line to the appropriate section of the instruction file (within an "AI Behavior Rules" section if one exists, otherwise at the end).

6. **On decline** — output the reference line for the user to add manually and exit without modifying any files.
