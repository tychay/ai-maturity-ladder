# AI Maturity Ladder

A portable methodology package for directing AI coding assistants (Claude Code) to **graduate work away from the AI** — reducing token usage, improving precision, and keeping the human in a deliberate practice loop.

## What's in this package

- **[`CLAUDE-RULES.md`](CLAUDE-RULES.md)** — Operational rules to reference from your project's `CLAUDE.md`. This is what the AI reads at session start.
- **[`assets/ai-maturity-ladder.md`](assets/ai-maturity-ladder.md)** — The full philosophy: ladder levels, Workflow Automation Framework, AI Justification Test, triggers, prescriptions, and examples.
- **[`link-map.json`](link-map.json)** — Maps source wikilinks to portable equivalents (used during distillation from the authoritative vault doc).
- **`skills/graduation-suggest/`** — A skill that prompts the AI to suggest graduation opportunities.

## Installation

1. Add as a git submodule:
   ```bash
   git submodule add <repo-url> ai/maturity-ladder
   ```

2. Add to your project's `CLAUDE.md` in the AI Behavior Rules section:
   ```markdown
   - **AI Maturity Ladder:** See `ai/maturity-ladder/CLAUDE-RULES.md` for operational rules
     and `ai/maturity-ladder/assets/ai-maturity-ladder.md` for the full philosophy.
   ```

3. Initialize the submodule when cloning:
   ```bash
   git submodule update --init
   ```

## How it works

The AI reads `CLAUDE-RULES.md` at session start for concise operational guidance. When facing ambiguous graduation decisions, it reads the full philosophy in `assets/`. The `graduation-suggest` skill is auto-discovered by Claude Code and provides proactive graduation recommendations.

## Updating

The authoritative source is the vault document. Changes flow one-way: vault → distillation → this package. Use the `/revise-maturity-ladder` skill (in the parent project, not this submodule) to run the revision loop.
