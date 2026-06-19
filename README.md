# AI Maturity Ladder

A Claude Code plugin that directs AI coding assistants to **graduate work away from the AI** — reducing token usage, improving precision, and keeping the human in a deliberate practice loop.

## What's in this plugin

- **[`CLAUDE-RULES.md`](CLAUDE-RULES.md)** — Operational rules injected into your project's instruction file by the setup skill.
- **[`assets/ai-maturity-ladder.md`](assets/ai-maturity-ladder.md)** — The full philosophy: ladder levels, Workflow Automation Framework, AI Justification Test, triggers, prescriptions, and examples.
- **`skills/graduation-suggest/`** — Proactively suggests when repeated patterns are ready to graduate.
- **`skills/setup/`** — Wires the maturity ladder into your project's instruction file.

## Installation

### Claude Code

Install from the repo:
```bash
claude plugin install tychay/ai-maturity-ladder
```

Then run the setup skill to configure your project:
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

For contributors developing this plugin:

```bash
# Test locally without installing
claude --plugin-dir ./path/to/ai-maturity-ladder

# Validate the plugin structure
claude plugin validate ./path/to/ai-maturity-ladder
```

## Updating

The authoritative source is the vault document. Changes flow one-way: vault → distillation → this package.
