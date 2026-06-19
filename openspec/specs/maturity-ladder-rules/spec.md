### Requirement: CLAUDE.md contains operational maturity ladder rules
The CLAUDE.md file SHALL contain a "Maturity ladder for automation" section within "AI Behavior Rules" that provides concise, actionable instructions for applying the AI Maturity Ladder. The section SHALL reference the full philosophy document for deeper context.

#### Scenario: Claude reads CLAUDE.md at session start
- **WHEN** Claude Code loads the project and reads CLAUDE.md
- **THEN** the maturity ladder section provides enough operational rules to guide behavior without requiring Claude to read the full philosophy doc

#### Scenario: Claude encounters an ambiguous graduation decision
- **WHEN** Claude is unsure whether a step should be graduated or which level applies
- **THEN** Claude can read the referenced philosophy doc for the full framework (WAF, AI Justification Test, examples)

### Requirement: Graduation progression is defined
The rules SHALL define the graduation progression as: act → skill → decomposed sub-skills → workflow (code orchestration) → tools (code execution).

#### Scenario: Claude notices a repeated pattern
- **WHEN** Claude observes the same sequence of actions being performed repeatedly
- **THEN** Claude suggests graduating it to the next level in the progression

### Requirement: AI Justification Test is stated as graduation criterion
The rules SHALL include the three-case test for when AI reasoning is justified in a workflow step: (1) unstructured→structured, (2) structured→unstructured, (3) conditions requiring semantic judgment. If a step does not meet any case, it should be code.

#### Scenario: Claude evaluates whether a step needs AI
- **WHEN** Claude is considering whether a workflow step can be graduated to deterministic code
- **THEN** Claude applies the three-case test: if the step doesn't require structuring, composing, or judging, it should be code

### Requirement: Code-mode principle is stated
The rules SHALL instruct Claude to always wrap external service calls (MCP or otherwise) as typed TypeScript APIs to enable tool-chaining without AI mediation.

#### Scenario: Claude makes an MCP tool call
- **WHEN** Claude uses an MCP tool or external service
- **THEN** Claude recognizes this as a graduation candidate for a code-mode wrapper and may suggest it

#### Scenario: Claude encounters a non-MCP external service
- **WHEN** a workflow involves calling an external API, AppleScript, CLI tool, or other non-MCP service repeatedly
- **THEN** Claude applies the same code-mode principle (typed TypeScript wrapper) as it would for MCP

### Requirement: Trigger axis is stated as separate from the ladder
The rules SHALL define the trigger graduation path (manual → Claude Code hook → system event) as a separate axis from the work-graduation ladder.

#### Scenario: A workflow is fully graduated but still manually invoked
- **WHEN** a tool/workflow is entirely deterministic code but requires the user to invoke it
- **THEN** Claude may suggest graduating the trigger (to hook or cron) as a separate step

### Requirement: OpenSpec/SDD instruction is preserved
The rules SHALL instruct Claude to use OpenSpec/SDD when building tools at any level.

#### Scenario: Claude is about to create a new tool
- **WHEN** Claude is implementing a new tool or graduating a workflow into code
- **THEN** Claude uses OpenSpec spec-driven development

### Requirement: Graduation suggestion instruction is preserved
The rules SHALL instruct Claude to proactively suggest when something is ready to graduate up the ladder.

#### Scenario: Claude detects graduation opportunity
- **WHEN** Claude observes a pattern that matches graduation criteria (repeated actions, high token cost for mechanical work, MCP calls that could be wrapped)
- **THEN** Claude suggests the graduation and which level it would move to
