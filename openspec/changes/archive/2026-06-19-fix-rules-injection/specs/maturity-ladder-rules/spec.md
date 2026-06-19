## MODIFIED Requirements

### Requirement: Operational rules are delivered via .claude/rules/ symlink
The plugin SHALL deliver its operational maturity ladder rules into a project's session context via a symlink in `.claude/rules/` pointing at the plugin's `rules/maturity-ladder.md` file. The rules SHALL NOT require a path reference in CLAUDE.md.

#### Scenario: Rules are loaded every session automatically
- **WHEN** Claude Code starts a session in a project where `/aiml:setup` has been run
- **THEN** the maturity ladder rules are present in session context (loaded via `.claude/rules/aiml-maturity-ladder.md` symlink)

#### Scenario: Rule updates propagate without re-running setup
- **WHEN** the plugin's `rules/maturity-ladder.md` is updated (via `revise-maturity-ladder` or plugin update)
- **THEN** the next session in any project with the symlink picks up the updated content automatically

#### Scenario: Claude encounters an ambiguous graduation decision
- **WHEN** Claude is unsure whether a step should be graduated or which level applies
- **THEN** the rules direct Claude to invoke `/aiml:understand-ladder` for the full philosophy

### Requirement: Graduation suggestion instruction is preserved
The rules SHALL instruct Claude to proactively suggest when something is ready to graduate up the ladder, referencing `/aiml:graduation-suggest` by its namespaced command.

#### Scenario: Claude detects graduation opportunity
- **WHEN** Claude observes a pattern that matches graduation criteria
- **THEN** it suggests the graduation and which level it would move to

## REMOVED Requirements

### Requirement: CLAUDE.md contains operational maturity ladder rules
**Reason**: Replaced by `.claude/rules/` symlink mechanism. CLAUDE.md no longer needs to reference the plugin's rules file by path — rules are auto-loaded from `.claude/rules/`.
**Migration**: Delete the maturity-ladder reference lines from CLAUDE.md and run `/aiml:setup` to install the symlink.
