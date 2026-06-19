## ADDED Requirements

### Requirement: Skills avoid hardcoded tool-specific invocation syntax
Skill SKILL.md files SHALL NOT use tool-specific slash command syntax when referencing other skills or themselves in instructional prose. They SHALL use plain names or descriptive references instead.

#### Scenario: Skill references another skill
- **WHEN** a skill's SKILL.md needs to mention another skill in the same plugin
- **THEN** it uses the skill name without assuming a specific invocation prefix

### Requirement: Skills use relative file paths for internal references
Skill SKILL.md files SHALL reference other files in the plugin using relative paths from the skill's location.

#### Scenario: graduation-suggest references the philosophy doc
- **WHEN** the graduation-suggest skill needs to point the agent at the full philosophy
- **THEN** it uses `../../assets/ai-maturity-ladder.md` (relative from its SKILL.md location)

### Requirement: Skills include a description field in YAML frontmatter
Every SKILL.md SHALL include a `description` field in its YAML frontmatter that summarizes the skill's purpose in one sentence.

#### Scenario: Tool indexes available skills
- **WHEN** a coding agent scans available skills and reads YAML frontmatter
- **THEN** the `description` field provides enough context to decide whether to invoke the skill

### Requirement: Skills do not reference tool-specific APIs in prose
Skill SKILL.md prose SHALL NOT name tool-specific APIs in their instructional text. They SHALL describe the desired behavior generically.

#### Scenario: Skill needs interactive confirmation
- **WHEN** a skill's instructions need the agent to ask the user a question
- **THEN** the prose says "ask the user for confirmation" not "use AskUserQuestion"
