## 1. Rules source file

- [x] 1.1 Create `rules/maturity-ladder.md` with operational rules content (6 bullets from `CLAUDE-RULES.md` plus a line directing Claude to `/aiml:understand-ladder` for the full philosophy)
- [x] 1.2 Verify the rules file is self-contained and doesn't reference paths outside the plugin

## 2. bin/setup-rules script

- [x] 2.1 Create `bin/setup-rules` as an executable shell script with shebang
- [x] 2.2 Implement: create `.claude/rules/` in `$CLAUDE_PROJECT_DIR` (or current directory) if it doesn't exist
- [x] 2.3 Implement: create symlink `.claude/rules/aiml-maturity-ladder.md` → `$CLAUDE_PLUGIN_ROOT/rules/maturity-ladder.md`
- [x] 2.4 Implement: idempotency check — if symlink exists and points to correct target, report "already configured"
- [x] 2.5 Implement: cross-platform detection — report clear error on Windows if symlink creation fails
- [x] 2.6 Make script executable (`chmod +x`)

## 3. Setup command

- [x] 3.1 Create `commands/setup.md` with frontmatter (name, description, allowed-tools for Bash)
- [x] 3.2 Write command instructions: detect environment (CLAUDE.md vs AGENTS.md), confirm with user, run `setup-rules`, report result
- [x] 3.3 Remove `skills/setup/SKILL.md`

## 4. Remove CLAUDE-RULES.md

- [x] 4.1 Delete `CLAUDE-RULES.md` from the plugin root
- [x] 4.2 Update any internal references (e.g. README.md, CLAUDE-RULES.md mentions in other docs)

## 5. Create understand-ladder skill

- [x] 5.1 Create `skills/understand-ladder/SKILL.md` with frontmatter — instructs Claude to read `assets/ai-maturity-ladder.md` and have the philosophy available in context
- [x] 5.2 Move `assets/ai-maturity-ladder.md` from the plugin root into `skills/understand-ladder/assets/ai-maturity-ladder.md`
- [x] 5.3 Remove the now-empty `assets/` directory at the plugin root (or repurpose if other files remain)

## 6. Update graduation-suggest skill

- [x] 6.1 Update `skills/graduation-suggest/SKILL.md` to invoke `/aiml:understand-ladder` instead of referencing `../../assets/ai-maturity-ladder.md` directly
- [x] 6.2 Remove the relative path reference to the old assets location

## 7. Validation

- [x] 7.1 Run `setup-rules` in the parent project (`tychay-alien-intelligence`) and verify `.claude/rules/aiml-maturity-ladder.md` symlink is created and resolves
- [ ] 7.2 Start a new Claude Code session and verify the maturity ladder rules appear in context
- [x] 7.3 Delete stale CLAUDE.md lines 75-76 and line 207 in the parent project
- [ ] 7.4 Verify `/aiml:setup` command is discoverable and runs the bin script
- [x] 7.5 Run `setup-rules` a second time to verify idempotency
- [ ] 7.6 Invoke `/aiml:understand-ladder` and verify the full philosophy loads into context
- [ ] 7.7 Invoke `/aiml:graduation-suggest` and verify it uses understand-ladder (not a direct asset path)
