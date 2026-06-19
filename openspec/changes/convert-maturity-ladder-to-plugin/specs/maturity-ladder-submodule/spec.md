## MODIFIED Requirements

### Requirement: graduation-suggest skill exists
The plugin SHALL contain `skills/graduation-suggest/SKILL.md` that instructs the coding agent to proactively suggest graduation opportunities. The skill SHALL be discovered via the plugin system.

#### Scenario: Plugin is installed and user invokes skill
- **WHEN** the plugin is installed and the user invokes `/aiml:graduation-suggest`
- **THEN** the coding agent reads the skill and applies its instructions to suggest graduation opportunities

#### Scenario: Coding agent detects repeated pattern
- **WHEN** the coding agent observes repeated actions that match graduation criteria
- **THEN** it suggests graduating per the ladder levels described in assets/ai-maturity-ladder.md

### Requirement: Plugin lives at .claude/skills/aiml/
The development submodule SHALL be located at `.claude/skills/aiml/` which is the canonical plugin install location. It auto-loads as `aiml@skills-dir` with no separate install step needed.

#### Scenario: Project is cloned fresh
- **WHEN** a developer clones the project and runs `git submodule update --init`
- **THEN** the `.claude/skills/aiml/` directory is populated and the plugin auto-loads

#### Scenario: Plugin system is not available
- **WHEN** a developer's coding agent does not support plugins
- **THEN** the submodule still functions via direct file path references from the instruction file
