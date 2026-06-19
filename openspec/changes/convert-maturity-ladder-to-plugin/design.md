## Context

The `ai/maturity-ladder` submodule currently ships methodology (CLAUDE-RULES.md, philosophy doc, link-map) and one skill (`graduation-suggest`) as a plain git submodule. The skill has never been auto-discovered because it's at `skills/` instead of `.claude/skills/`. Claude Code has a plugin system (since Oct 2025) that provides proper skill discovery, namespacing, and a setup lifecycle. We adopt the plugin format while preserving cross-tool portability.

**Current structure:**
```
ai/maturity-ladder/
├── CLAUDE-RULES.md
├── README.md
├── assets/ai-maturity-ladder.md
├── link-map.json
├── skills/graduation-suggest/SKILL.md  ← never discovered
├── adr/
└── openspec/
```

**In-force ADRs (maturity-ladder scope):**
- 0002: Three-file split (CLAUDE-RULES.md / assets/ / README.md)
- 0005: graduation-suggest in submodule, revise-maturity-ladder in parent
- 0010: CLAUDE.md references by file path, not wikilink

## Goals / Non-Goals

**Goals:**
- Fix broken skill discovery (graduation-suggest actually works after this)
- Adopt Claude Code plugin conventions (manifest, namespaced skills, setup lifecycle)
- Preserve submodule-as-git-repo portability (still clonable, still works without plugin system)
- Design skills to be lightly tool-agnostic (no hard Claude-specific tool references in prose)
- Stub environment-detection in setup skill for future cross-tool support
- Distribute via marketplace (`claude plugin install`); remove submodule from originating project

**Non-Goals:**
- Building actual OpenCode plugin support (just awareness in design)
- Migrating the parent project from CLAUDE.md to AGENTS.md
- Changing the maturity-ladder philosophy content or rules
- Modularizing evaluators (that's the openspec-adr plugin, separate change)
- Remote marketplace publishing (local marketplace only; remote registry is future)

## Decisions

### D1: Plugin manifest at `.claude-plugin/plugin.json`

Only required field is `name`. We use `"name": "aiml"` — this IS the namespace prefix for skills (no separate alias field exists; feature request #23589 is open but unimplemented).

```json
{
  "name": "aiml",
  "displayName": "AI Maturity Ladder",
  "version": "1.0.0",
  "description": "AI Maturity Ladder — graduation methodology for AI-assisted development",
  "author": { "name": "tychay" },
  "repository": "https://github.com/tychay/ai-maturity-ladder"
}
```

Skills at `skills/` are auto-discovered — no `"skills"` field needed in manifest.

### D2: Skills stay at `skills/`, auto-discovered by plugin system

Plugin convention uses `skills/<name>/SKILL.md` within the plugin directory. The bare `skills/` path is correct for plugins.

### D3: Setup skill handles CLAUDE-RULES.md wiring

A new `skills/setup/SKILL.md` skill (invoked as `/aiml:setup`) will:
1. Detect the host environment (check for CLAUDE.md, AGENTS.md, .opencode/)
2. Ask the user for confirmation before modifying files
3. Inject the reference line into the chosen instruction file
4. Handle idempotency (skip if already configured)

Environment detection for non-Claude paths stubs out with "not yet supported."

### D4: graduation-suggest gets light portability edits

- Add `description` field in YAML frontmatter (tools use this for discovery)
- Use relative path references (`../../assets/ai-maturity-ladder.md`)
- Avoid referencing Claude-specific tools in the skill prose

### D5: Plugin distributed via marketplace (not `.claude/skills/`)

The original plan was to move the submodule to `.claude/skills/aiml/` for auto-loading. This hit a trust-gate bug in Claude Code: the trust dialog never re-presents after dismissal, making plugins at `.claude/skills/` permanently unloadable. The actual resolution: the `ai/maturity-ladder` submodule was removed from the originating project. The plugin is distributed via marketplace (`claude plugin install`) using a local marketplace with symlinked cache for development. The repo remains a standalone git repo consumable as a raw submodule for projects that want path-based references — but NOT at `.claude/skills/` due to the trust-gate bug. ADR-0001 is superseded by ADR-0011.

### D6: ADR-0010 path resolution adapts

ADR-0010 says CLAUDE.md references by file path. The setup skill resolves the correct path at install time based on where the plugin actually lives. With marketplace distribution, the plugin cache location is resolved at runtime rather than being a fixed submodule path.

## Risks / Trade-offs

**[Plugin system instability]** → Manifest format may change. Mitigation: minimal manifest (one JSON file) — easy to update.

**[No alias support]** → The `name` field IS the namespace. We chose `"aiml"` (short) rather than `"ai-maturity-ladder"` (long). Tradeoff: less descriptive in isolation, but much better UX for frequent invocation.

**[Cross-tool stub is dead code]** → The AGENTS.md / .opencode/ detection branches do nothing today. Mitigation: 2-3 lines each. Remove if portability goal is abandoned.

**[ADR-0005 tension]** → ADR-0005 says graduation-suggest is "auto-discovered." It now truly is via the plugin system, but through a different mechanism than 0005 assumed. The spirit is preserved.
