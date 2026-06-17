# Active ADRs

Currently-in-force architectural decisions for ai/maturity-ladder.

- [0001-ai-namespace-for-methodology](0001-ai-namespace-for-methodology.md) — ai/ is the top-level namespace for portable AI methodology submodules
- [0002-submodule-file-structure](0002-submodule-file-structure.md) — CLAUDE-RULES.md (operational), assets/ (philosophy), README.md (humans)
- [0003-vault-authoritative-one-way-distillation](0003-vault-authoritative-one-way-distillation.md) — Vault doc is single source of truth; submodule is derived via distillation
- [0004-link-map-wikilink-resolution](0004-link-map-wikilink-resolution.md) — link-map.json maps vault wikilinks to URLs/null/paths for reproducible distillation
- [0005-skill-portability-boundary](0005-skill-portability-boundary.md) — Portable skills in submodule; vault-dependent skills in parent project
- [0006-triggers-separate-axis](0006-triggers-separate-axis.md) — Triggers (what starts work) are orthogonal to the ladder (what does work)
- [0007-ai-justification-test](0007-ai-justification-test.md) — AI justified only for: structuring, composing, or judging; everything else graduates
- [0008-code-mode-is-concept-not-mcp](0008-code-mode-is-concept-not-mcp.md) — Code-mode = typed API wrapper for any external service, not MCP-specific
- [0009-token-cost-as-ill-posedness-proxy](0009-token-cost-as-ill-posedness-proxy.md) — Tokens proxy ill-posedness (precision), not just cost
- [0010-claude-md-uses-file-paths](0010-claude-md-uses-file-paths.md) — CLAUDE.md references use resolvable file paths, never wikilinks
