## ADDED Requirements

### Requirement: Plugin manifest exists at .claude-plugin/plugin.json
The submodule SHALL contain a `.claude-plugin/plugin.json` file at its root that declares the plugin identity, version, and description.

#### Scenario: Claude Code discovers the plugin
- **WHEN** a user runs `claude plugin install` pointing at the repo URL
- **THEN** Claude Code reads `.claude-plugin/plugin.json` and registers the plugin with its declared name as the skill namespace

#### Scenario: Plugin manifest contains required fields
- **WHEN** `.claude-plugin/plugin.json` is parsed
- **THEN** it contains at minimum: `name` (string, the namespace prefix for all skills)

### Requirement: Plugin name enables namespaced skill invocation
The manifest `name` field SHALL determine the namespace prefix for all skills in the plugin.

#### Scenario: User invokes a namespaced skill
- **WHEN** the user types `/aiml:graduation-suggest`
- **THEN** Claude Code resolves `aiml` to the plugin and invokes the `graduation-suggest` skill from its skills directory

### Requirement: Plugin coexists with raw submodule usage
The `.claude-plugin/` directory SHALL NOT interfere with using the repository as a plain git submodule without the plugin system.

#### Scenario: Submodule used without plugin install
- **WHEN** a project includes the repo as a git submodule but does not run `plugin install`
- **THEN** CLAUDE-RULES.md and assets/ remain readable by path reference from the instruction file
