## Why

The `ai/maturity-ladder` submodule has a broken skill (`graduation-suggest`) that was never auto-discovered because it lives at `skills/` instead of `.claude/skills/`. Rather than just fixing the path, this is the right time to convert the submodule into a proper Claude Code plugin — the standard distribution mechanism — with an eye toward cross-tool portability (OpenCode and future agents). The plugin format gives us: marketplace discoverability, namespaced skills, a setup command for wiring into projects, and (ironically) the correct skill path for free since plugins use bare `skills/`.

## What Changes

- Convert `ai/maturity-ladder` git submodule into a Claude Code plugin structure (adding `.claude-plugin/plugin.json` manifest)
- Move skill from `skills/graduation-suggest/SKILL.md` → plugin-standard `skills/graduation-suggest/SKILL.md` (path stays the same; plugin discovery handles it)
- Add a `setup` skill that wires `CLAUDE-RULES.md` into the host project's instruction file
- Lightly touch skill content to avoid Claude-specific tool references where possible (portable-by-default)
- Stub environment-detection branching in setup skill (detect CLAUDE.md vs AGENTS.md) as a no-op path for future cross-tool support
- The submodule remains a git submodule for project-scoped installs; plugin packaging is additive
- Move submodule from `ai/maturity-ladder` to `.claude/skills/aiml/` — the canonical plugin install location

## Capabilities

### New Capabilities
- `plugin-manifest`: The `.claude-plugin/plugin.json` manifest that makes the submodule discoverable as a Claude Code plugin (name, version, description)
- `plugin-setup-skill`: A `/aiml:setup` skill that detects the host environment and wires CLAUDE-RULES.md into the project's instruction file
- `portable-skill-pattern`: Design pattern for writing skills that self-detect their host environment (CLAUDE.md vs AGENTS.md vs .opencode/) and adapt behavior accordingly

### Modified Capabilities
- `maturity-ladder-submodule`: The submodule gains plugin structure alongside its existing layout; graduation-suggest skill gets lightly revised for tool-agnosticism

## Impact

- **Submodule location**: Moves from `ai/maturity-ladder/` to `.claude/skills/aiml/`
- **Parent project CLAUDE.md**: Path references updated to new location
- **ADR-0001**: Superseded by new ADR-0011 (plugin over ai/ namespace)
- **Existing consumers**: Must re-install via `claude plugin install` (or update submodule path)
