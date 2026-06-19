## ADDED Requirements

### Requirement: Setup skill exists at skills/setup/SKILL.md
The plugin SHALL contain a `skills/setup/SKILL.md` that handles first-time integration of the maturity ladder into a host project.

#### Scenario: User invokes setup
- **WHEN** the user invokes `/aiml:setup`
- **THEN** the skill begins the environment detection and wiring process

### Requirement: Setup detects host environment
The setup skill SHALL check for the existence of instruction files to determine the host coding agent.

#### Scenario: CLAUDE.md exists
- **WHEN** the setup skill detects a `CLAUDE.md` file in the project root
- **THEN** it proceeds with the Claude Code integration path

#### Scenario: AGENTS.md exists but no CLAUDE.md
- **WHEN** the setup skill detects an `AGENTS.md` but no `CLAUDE.md`
- **THEN** it reports that cross-tool support is not yet implemented and exits gracefully

#### Scenario: No recognized instruction file exists
- **WHEN** the setup skill finds neither `CLAUDE.md` nor `AGENTS.md`
- **THEN** it asks the user what to do (create CLAUDE.md, or abort)

### Requirement: Setup asks permission before modifying files
The setup skill SHALL ask the user for confirmation before writing to the host project's instruction file.

#### Scenario: User approves injection
- **WHEN** the setup skill presents the proposed addition and the user approves
- **THEN** it appends the maturity ladder reference line to the appropriate section

#### Scenario: User declines injection
- **WHEN** the user declines the proposed modification
- **THEN** the setup skill outputs the reference line for the user to add manually and exits without modifying files

### Requirement: Setup is idempotent
The setup skill SHALL detect if the maturity ladder is already referenced in the instruction file and skip injection if so.

#### Scenario: Instruction file already references maturity ladder
- **WHEN** the instruction file already contains a line referencing `maturity-ladder/CLAUDE-RULES.md` or `aiml/CLAUDE-RULES.md`
- **THEN** the setup skill reports "already configured" and exits without modification
