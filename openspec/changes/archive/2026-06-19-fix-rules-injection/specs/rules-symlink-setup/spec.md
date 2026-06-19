## ADDED Requirements

### Requirement: bin/setup-rules script exists and is executable
The plugin SHALL contain a `bin/setup-rules` script that creates a symlink from the project's `.claude/rules/` directory to the plugin's rules source file.

#### Scenario: Script creates symlink on macOS/Linux
- **WHEN** `setup-rules` is invoked in a project directory
- **THEN** it creates `.claude/rules/` if it does not exist and creates a symlink `.claude/rules/aiml-maturity-ladder.md` pointing at `$CLAUDE_PLUGIN_ROOT/rules/maturity-ladder.md`

#### Scenario: Script is idempotent
- **WHEN** `setup-rules` is invoked and the symlink already exists pointing at the correct target
- **THEN** it reports "already configured" and exits successfully without modification

#### Scenario: Script handles Windows
- **WHEN** `setup-rules` is invoked on Windows without symlink permissions
- **THEN** it reports a clear error message explaining that Developer Mode or elevation is required

#### Scenario: Script uses CLAUDE_PLUGIN_ROOT for path resolution
- **WHEN** `setup-rules` constructs the symlink target path
- **THEN** it uses `$CLAUDE_PLUGIN_ROOT` to resolve the plugin's installation directory, making it portable across different install locations

### Requirement: Rules source file exists at rules/maturity-ladder.md
The plugin SHALL contain a `rules/maturity-ladder.md` file with the operational rules content (the symlink target).

#### Scenario: Rules file contains all operational rules
- **WHEN** the rules file is read by Claude Code via `.claude/rules/` symlink
- **THEN** it contains the graduation progression, AI Justification Test, code-mode principle, trigger axis, OpenSpec/SDD instruction, and graduation suggestion instruction

#### Scenario: Rules file references the understand-ladder skill for philosophy
- **WHEN** Claude encounters an ambiguous graduation decision
- **THEN** the rules file instructs Claude to invoke `/aiml:understand-ladder` which loads the full AI Maturity Ladder philosophy into context
