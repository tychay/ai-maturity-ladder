## Why

The plugin's setup skill injects a file-path reference into CLAUDE.md pointing at `CLAUDE-RULES.md`. This worked when the plugin lived as a submodule inside the project tree. With marketplace distribution, the plugin is outside the project — there's no stable relative path from the project root to the plugin's files. The CLAUDE.md reference is broken on every install.

## What Changes

- Add a `bin/setup-rules` script that creates `.claude/rules/` in the project (if needed) and symlinks the plugin's rules file into it using `$CLAUDE_PLUGIN_ROOT` for path resolution
- Move operational rules content from `CLAUDE-RULES.md` into a rules source file within the plugin (to be symlinked)
- Create a new `skills/understand-ladder/` skill that loads `assets/ai-maturity-ladder.md` (the full philosophy) into context on demand. Move `assets/ai-maturity-ladder.md` into this skill's own assets directory.
- The rules file includes a rule: "if you need deeper understanding of the maturity ladder, invoke `/aiml:understand-ladder`"
- Update `skills/graduation-suggest/` to invoke `/aiml:understand-ladder` instead of referencing `../../assets/ai-maturity-ladder.md` directly
- Replace `skills/setup/SKILL.md` with `commands/setup.md` that confirms with the user then runs `setup-rules`
- Remove `CLAUDE-RULES.md` (replaced by the rules source file)

## Capabilities

### New Capabilities
- `rules-symlink-setup`: A `bin/setup-rules` script + `commands/setup.md` command that installs the plugin's rules into a project's `.claude/rules/` via symlink, using `$CLAUDE_PLUGIN_ROOT` for portable path resolution
- `understand-ladder`: A new skill that loads the full AI Maturity Ladder philosophy into context on demand. Houses `assets/ai-maturity-ladder.md` in its own assets directory.

### Modified Capabilities
- `plugin-setup-skill`: Replaced by `commands/setup.md` — no longer injects a path into CLAUDE.md; instead runs the bin script to create the symlink
- `maturity-ladder-rules`: Rules content moves from `CLAUDE-RULES.md` to a file designed to be symlinked into `.claude/rules/`. Includes a rule pointing at `/aiml:understand-ladder` for deeper context.
- `graduation-suggest`: No longer references `../../assets/ai-maturity-ladder.md` directly — invokes `/aiml:understand-ladder` instead

### Removed Capabilities
- `CLAUDE-RULES.md` as a standalone referenced file — replaced by the symlinked rules mechanism

## Impact

- Projects using the plugin must run `/aiml:setup` once after install (same as before, different mechanism)
- The symlink means rule updates propagate automatically — no re-running setup after `revise-maturity-ladder`
- `CLAUDE-RULES.md` is removed; consumers who referenced it by path must re-run setup
- Windows users need Developer Mode or elevated prompt for symlink creation (cross-platform consideration)
- The parent project (`tychay-alien-intelligence`) will delete its stale CLAUDE.md lines 75-76 and line 207, then run `/aiml:setup` to verify
