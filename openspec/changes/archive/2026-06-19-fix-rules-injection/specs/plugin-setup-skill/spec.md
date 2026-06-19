## MODIFIED Requirements

### Requirement: Setup command exists and handles first-time integration
The plugin SHALL contain a `commands/setup.md` command (invoked as `/aiml:setup`) that handles first-time integration of the maturity ladder rules into a host project by running the `setup-rules` bin script.

#### Scenario: User invokes setup
- **WHEN** the user invokes `/aiml:setup`
- **THEN** the command asks for confirmation before making changes to the project

#### Scenario: User approves setup
- **WHEN** the user confirms they want to install the rules
- **THEN** the command runs `setup-rules` (from the plugin's `bin/` directory, available on PATH) and reports the result

#### Scenario: User declines setup
- **WHEN** the user declines the confirmation
- **THEN** the command exits without modification and explains what manual steps would be needed

#### Scenario: Setup detects non-Claude environment
- **WHEN** the command detects AGENTS.md without CLAUDE.md, or a `.opencode/` directory
- **THEN** it reports that this environment is not yet supported and exits gracefully

## REMOVED Requirements

### Requirement: Setup skill exists at skills/setup/SKILL.md
**Reason**: Replaced by `commands/setup.md`. Setup is an imperative one-time action (command), not a pattern-matched behavior (skill). The new command runs `bin/setup-rules` instead of injecting a path into CLAUDE.md.
**Migration**: Run `/aiml:setup` — the command is now at `commands/setup.md` and creates a symlink instead of editing CLAUDE.md.

### Requirement: Setup asks permission before modifying files
**Reason**: Merged into the new setup command requirement above (confirmation step preserved).
**Migration**: Behavior is unchanged — the new command still confirms before acting.

### Requirement: Setup is idempotent
**Reason**: Moved to the `bin/setup-rules` script's idempotency requirement (in rules-symlink-setup spec).
**Migration**: Idempotency is preserved in the bin script.
