## MODIFIED Requirements

### Requirement: graduation-suggest skill exists
The plugin SHALL contain `skills/graduation-suggest/SKILL.md` that instructs the coding agent to proactively suggest graduation opportunities. The skill SHALL be discovered via the plugin system.

#### Scenario: Plugin is installed and user invokes skill
- **WHEN** the plugin is installed and the user invokes `/aiml:graduation-suggest`
- **THEN** the coding agent reads the skill and applies its instructions to suggest graduation opportunities

#### Scenario: Coding agent detects repeated pattern
- **WHEN** the coding agent observes repeated actions that match graduation criteria
- **THEN** it suggests graduating per the ladder levels described in assets/ai-maturity-ladder.md

### Requirement: Plugin distributed via marketplace
The plugin SHALL be installable via `claude plugin install` from a local or remote marketplace. The `.claude/skills/aiml` submodule was removed from the originating project due to a trust-gate bug (trust dialog never re-presents after dismissal, making plugins at `.claude/skills/` permanently unloadable).

#### Scenario: Plugin is installed via marketplace
- **WHEN** a developer runs `claude plugin install aiml` (or equivalent marketplace command)
- **THEN** the plugin is installed to the local plugin cache and its skills are discoverable

#### Scenario: Plugin used as raw submodule (non-skills path)
- **WHEN** a project includes the plugin repo as a git submodule at a path OTHER than `.claude/skills/`
- **THEN** the submodule still functions via direct file path references from the instruction file
- **NOTE** The submodule MUST NOT be placed at `.claude/skills/` due to the trust-gate bug

#### Scenario: Plugin system is not available
- **WHEN** a developer's coding agent does not support plugins
- **THEN** the submodule still functions via direct file path references from the instruction file
