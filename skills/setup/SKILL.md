---
description: Install the AI Maturity Ladder rules into the current project
disable-model-invocation: true
allowed-tools: Bash(setup-rules) AskUserQuestion
---

# Setup

Install the AI Maturity Ladder operational rules into this project.

## Steps

1. **Detect environment** — check which instruction files exist at the project root:
   - `CLAUDE.md` exists → proceed
   - `AGENTS.md` (without CLAUDE.md) → report "AGENTS.md integration not yet supported" and exit
   - `.opencode/` directory → report "OpenCode integration not yet supported" and exit
   - None found → ask the user if they want to proceed anyway (rules will still load from `.claude/rules/`)

2. **Confirm with user** — explain that this will create a symlink in `.claude/rules/` pointing at the plugin's rules file. Ask for confirmation before proceeding.

3. **Run setup** — execute `setup-rules` to create the symlink.

4. **Report result** — tell the user what was done. If the project's CLAUDE.md has stale references to `.claude/skills/aiml/CLAUDE-RULES.md` or similar, suggest removing them (the rules now auto-load from `.claude/rules/`).
