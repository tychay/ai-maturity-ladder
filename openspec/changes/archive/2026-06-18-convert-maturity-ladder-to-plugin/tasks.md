## 1. Plugin Manifest

- [x] 1.1 Create `.claude-plugin/plugin.json` at submodule root with name ("aiml"), version, description, author
- [x] 1.2 Verify manifest schema against current Claude Code plugin docs (`claude plugin validate` passes)

## 2. Graduation-Suggest Skill Portability

- [x] 2.1 Add YAML frontmatter to `skills/graduation-suggest/SKILL.md` with `description` field
- [x] 2.2 Replace `/graduation-suggest` self-reference with namespaced form in CLAUDE-RULES.md
- [x] 2.3 Replace `assets/ai-maturity-ladder.md` reference with relative path from skill location (`../../assets/ai-maturity-ladder.md`)
- [x] 2.4 Review skill prose for any other tool-specific API references and genericize

## 3. Setup Skill

- [x] 3.1 Create `skills/setup/SKILL.md` with YAML frontmatter (description)
- [x] 3.2 Write environment detection phase (check for CLAUDE.md, AGENTS.md, .opencode/)
- [x] 3.3 Write CLAUDE.md integration path (resolve relative path to CLAUDE-RULES.md, propose injection, handle approval/decline)
- [x] 3.4 Add idempotency check (detect if maturity-ladder already referenced)
- [x] 3.5 Stub AGENTS.md / .opencode/ paths with "not yet supported" message

## 4. README and Documentation

- [x] 4.1 Update README.md to document plugin installation (primary) alongside raw submodule usage
- [x] 4.2 Update CLAUDE-RULES.md reference to graduation-suggest to use namespaced form (`/aiml:graduation-suggest`)

## 5. Submodule Relocation

- [x] 5.1 Remove `ai/maturity-ladder` submodule from originating project; set up marketplace distribution (local marketplace with symlinked cache for dev). Note: `.claude/skills/aiml/` path was abandoned due to trust-gate bug.
- [x] 5.2 Update parent CLAUDE.md path references
- [x] 5.3 Supersede ADR-0001, create ADR-0011

## 6. Validation

- [x] 6.1 `claude plugin validate` passes at new location
- [ ] 6.2 Verify `/aiml:graduation-suggest` is discoverable and invocable after install (validate via `claude plugin install` from local marketplace, not submodule path)
- [ ] 6.3 Run `/aiml:setup` and confirm it correctly detects CLAUDE.md and proposes the right reference line (validate via marketplace install path, not submodule path)
- [x] 6.4 Verify raw submodule path still works (CLAUDE-RULES.md accessible)
