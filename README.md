# AI Maturity Ladder

A Claude Code plugin that directs AI coding assistants to **graduate work away from the AI** — reducing token usage, improving precision, and keeping the human in a deliberate practice loop.

## What's in this plugin

- **[`CLAUDE-RULES.md`](CLAUDE-RULES.md)** — Operational rules injected into your project's instruction file by the setup skill.
- **[`assets/ai-maturity-ladder.md`](assets/ai-maturity-ladder.md)** — The full philosophy: ladder levels, Workflow Automation Framework, AI Justification Test, triggers, prescriptions, and examples.
- **`skills/graduation-suggest/`** — Proactively suggests when repeated patterns are ready to graduate.
- **`skills/setup/`** — Wires the maturity ladder into your project's instruction file.

## Installation

### Claude Code

Register the marketplace and install:
```bash
claude plugin marketplace add tychay/ai-maturity-ladder
claude plugin install aiml
```

### After install

Run the setup skill to configure your project:
```
/aiml:setup
```

This detects your instruction file (CLAUDE.md) and adds the reference line automatically.

### Other coding agents

OpenCode and other agent support is not yet implemented. The setup skill will report this if it detects a non-Claude environment. The underlying files (CLAUDE-RULES.md, assets/) are plain markdown and can be referenced manually from any agent's instruction file.

## Skills

| Skill | Description |
|-------|-------------|
| `/aiml:setup` | Wire the maturity ladder into your project |
| `/aiml:graduation-suggest` | Suggest when patterns are ready to graduate |

## How it works

After setup, the AI reads `CLAUDE-RULES.md` at session start for concise operational guidance. When facing ambiguous graduation decisions, it reads the full philosophy in `assets/`. The graduation-suggest skill provides proactive recommendations when it notices repeated patterns.

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
