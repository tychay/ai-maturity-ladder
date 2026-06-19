## ADDED Requirements

### Requirement: understand-ladder skill exists
The plugin SHALL contain a `skills/understand-ladder/SKILL.md` skill that loads the full AI Maturity Ladder philosophy into Claude's context on demand.

#### Scenario: User or another skill invokes understand-ladder
- **WHEN** `/aiml:understand-ladder` is invoked
- **THEN** Claude reads `assets/ai-maturity-ladder.md` from the skill's assets directory and has the full philosophy available in context

#### Scenario: graduation-suggest needs the philosophy
- **WHEN** `/aiml:graduation-suggest` needs the full ladder philosophy to make a recommendation
- **THEN** it invokes `/aiml:understand-ladder` rather than referencing a relative file path directly

### Requirement: Philosophy doc lives in the understand-ladder skill's assets
The file `ai-maturity-ladder.md` (the full philosophy) SHALL be located at `skills/understand-ladder/assets/ai-maturity-ladder.md` within the plugin.

#### Scenario: File is accessible to the skill
- **WHEN** the understand-ladder skill instructs Claude to read its assets
- **THEN** `assets/ai-maturity-ladder.md` is resolvable relative to the skill's SKILL.md location
