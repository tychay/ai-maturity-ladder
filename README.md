# AI Maturity Ladder

A Claude Code plugin that directs AI coding assistants to **graduate work away from the AI** — reducing token usage, improving precision, and keeping the human in a deliberate practice loop.

## What's in this plugin

- **`rules/maturity-ladder.md`** — Operational rules, symlinked into your project's `.claude/rules/` by the setup command.
- **`skills/understand-ladder/`** — Loads the full philosophy (AI Justification Test, Workflow Automation Framework, ladder levels) into context on demand.
- **`skills/graduation-suggest/`** — Proactively suggests when repeated patterns are ready to graduate.
- **`commands/setup.md`** — One-time setup: symlinks the rules into your project's `.claude/rules/`.

## Installation

### Claude Code

Register the marketplace and install:
```bash
claude plugin marketplace add tychay/ai-maturity-ladder
claude plugin install aiml
```

### After install

Run the setup command to install rules into your project:
```
/aiml:setup
```

This creates a symlink in `.claude/rules/` pointing at the plugin's rules file. Rules auto-load every session.

### Other coding agents

OpenCode and other agent support is not yet implemented. The setup command will report this if it detects a non-Claude environment.

## Commands & Skills

| Name | Type | Description |
|------|------|-------------|
| `/aiml:setup` | command | Install rules into your project (one-time) |
| `/aiml:understand-ladder` | skill | Load the full maturity ladder philosophy into context |
| `/aiml:graduation-suggest` | skill | Suggest when patterns are ready to graduate |

## How it works

After setup, the operational rules auto-load every session via `.claude/rules/`. When facing ambiguous graduation decisions, Claude invokes `/aiml:understand-ladder` for the full philosophy. The graduation-suggest skill provides proactive recommendations when it notices repeated patterns.

## Development

Assumes the local marketplace is already registered and the plugin is installed
with the cache symlinked to this source directory.

**Dev loop:**

1. Edit source files directly in this directory (symlinked cache means no reinstall or version bump needed locally)
2. Pick up changes: **Cmd-Shift-P > "Developer: Reload Window"** (VSCode) or relaunch the panel
3. Validate structure: `claude plugin validate ./path/to/ai-maturity-ladder`
4. For CLI-only testing, `claude --plugin-dir ./path/to/ai-maturity-ladder` also works (but does NOT apply in VSCode extension)

**Publishing changes to others:** Bump `version` in `plugin.json` before pushing — consumers pick up new versions via `claude plugin update`.

## Updating

The authoritative source is the vault document. Changes flow one-way: vault → distillation → this package.
