## Context

The plugin currently delivers operational rules via `CLAUDE-RULES.md` at the plugin root, with a setup skill that injects a path reference into the host project's CLAUDE.md. This worked when the plugin was a submodule inside the project tree (`.claude/skills/aiml/CLAUDE-RULES.md`). With marketplace distribution, the plugin lives outside the project at `~/.claude/plugins/cache/...` — no stable relative path exists from the project root.

Claude Code's `.claude/rules/` directory auto-loads `.md` files into every session's context. It supports symlinks (documented: "The `.claude/rules/` directory supports symlinks, so you can maintain a shared set of rules and link them into multiple projects"). Plugin `bin/` scripts are added to PATH and have access to `$CLAUDE_PLUGIN_ROOT` (the plugin's install directory).

**In-force ADRs:**
- 0011: Plugin distribution over ai/ namespace (marketplace, not submodule)
- 0010: CLAUDE.md references by file path (this is what's broken — we're replacing it)

## Goals / Non-Goals

**Goals:**
- Rules auto-load every session without a CLAUDE.md path reference
- Rule updates (via `revise-maturity-ladder`) propagate to all projects automatically — no re-running setup
- Setup runs once per machine, same as `claude plugin marketplace add`
- The philosophy doc (`assets/ai-maturity-ladder.md`) remains accessible on-demand via a skill
- Cross-platform: handle Windows symlink limitations gracefully

**Non-Goals:**
- Runtime enforcement via hooks (our rules are context/behavioral, not tool-call checks)
- Supporting the old `CLAUDE-RULES.md` path-reference model alongside the new one
- Modifying the graduation-suggest skill's logic (just its asset access pattern)
- Solving this for OpenCode or other agents (future concern)

## Decisions

### D1: Rules delivered via symlink into `.claude/rules/`

The `bin/setup-rules` script creates a symlink: `.claude/rules/aiml-maturity-ladder.md` → `$CLAUDE_PLUGIN_ROOT/rules/maturity-ladder.md`. The symlink means the project always reads the plugin's current content — updates via `revise-maturity-ladder` propagate instantly.

Alternative: copy the file (stale after updates). Rejected — defeats the reference model.
Alternative: `SessionStart` hook that copies fresh each session. Rejected — hooks run shell commands, they can't inject into LLM context; and copying on every session start is wasteful for a file that rarely changes.

### D2: Rules source file at `rules/maturity-ladder.md` in the plugin

The rules content (currently in `CLAUDE-RULES.md`) moves to `rules/maturity-ladder.md`. This file is what gets symlinked into projects. It contains the 6 operational rules plus a line directing Claude to invoke `/aiml:understand-ladder` when facing ambiguous graduation decisions.

Note: `rules/` is NOT a plugin-system feature (plugins don't auto-load from `rules/`). It's just a directory name — we chose it for clarity about the file's purpose. This convention is used by the community of plugin developers.

### D3: New `understand-ladder` skill owns the philosophy doc

A new skill `skills/understand-ladder/` exists solely to load the full AI Maturity Ladder philosophy into context on demand. The `assets/ai-maturity-ladder.md` file moves from the plugin root into this skill's assets directory (`skills/understand-ladder/assets/ai-maturity-ladder.md`). The skill's SKILL.md instructs Claude to read its `assets/ai-maturity-ladder.md` and have it available in context.

The rules file (`rules/maturity-ladder.md`) includes: "If you need deeper understanding of the maturity ladder levels, invoke `/aiml:understand-ladder`."

The `graduation-suggest` skill is updated to invoke `/aiml:understand-ladder` instead of referencing `../../assets/ai-maturity-ladder.md` directly. This removes the fragile relative path and gives each skill a single responsibility.

Alternative: keep philosophy in `graduation-suggest`. Rejected — overloads that skill with two jobs (knowing the philosophy AND suggesting graduations). Separation is cleaner.
Alternative: symlink `assets/ai-maturity-ladder.md` into `.claude/rules/`. Rejected — too heavy for every session; it's 3000+ words only needed for edge cases.
Alternative: have a agent handle ladder understanding. Rejected — this is a valid enhancement for the future.

### D4: Setup is `commands/setup.md` calling `bin/setup-rules`

The current `skills/setup/SKILL.md` is replaced by `commands/setup.md` (shows in `/` autocomplete, user-invoked). The command instructs Claude to confirm with the user, then run `setup-rules` (a bare command — `bin/` is on PATH). The setup command also handles the environment detection (CLAUDE.md vs AGENTS.md) for future portability.

Alternative: keep as a skill. Rejected — setup is an imperative one-time action, not a pattern-matched behavior.

### D5: `bin/setup-rules` handles cross-platform

The script checks for OS and:
- macOS/Linux: creates symlink via `ln -sf`
- Windows: attempts `mklink /D`, reports clear error if it fails (Developer Mode or elevation needed)
- Both: creates `.claude/rules/` directory if it doesn't exist
- Idempotent: if symlink already exists and points to the right target, reports "already configured"

### D6: `CLAUDE-RULES.md` removed

The file is deleted. It was an artifact of the submodule/path-reference era. Any consumer referencing it by path gets a clear error and must re-run `/aiml:setup`.

## Risks / Trade-offs

**[Symlink breakage on plugin update]** → If `claude plugin update` replaces the cache directory (breaking the existing symlink target), re-running `/aiml:setup` fixes it. Mitigation: the symlink points at `$CLAUDE_PLUGIN_ROOT/rules/...` which resolves through the cache symlink we already have. Since our dev setup uses a symlinked cache, the chain is stable.

**[Windows symlink friction]** → Windows requires Developer Mode or elevation. Mitigation: clear error message with instructions. This is the same constraint any cross-project symlink has on Windows.

**[`.claude/rules/` not gitignored by default]** → The symlink itself would be committed if `.claude/rules/` is tracked. The symlink target is an absolute path (machine-specific). Mitigation: the setup command adds `.claude/rules/aiml-maturity-ladder.md` to `.gitignore` (or recommends it). Alternatively, the project can gitignore all of `.claude/rules/` if rules are plugin-sourced.

**[ADR-0010 tension]** → ADR-0010 says "CLAUDE.md references by file path." We're moving away from that for plugin-delivered rules. The ADR's spirit (rules are readable by path) is preserved — they're at `.claude/rules/aiml-maturity-ladder.md`. The mechanism changed from "reference in CLAUDE.md" to "auto-loaded from `.claude/rules/`."

**[No uninstall script]** → There is no `bin/teardown-rules` or uninstall mechanism to remove the symlinks. If a user uninstalls the plugin, the dangling symlink in `.claude/rules/` will be harmlessly ignored (broken symlinks don't load). Manual cleanup is `rm .claude/rules/aiml-maturity-ladder.md`. Out of scope for this change.
