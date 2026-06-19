# 0011 — Claude Code plugin distribution over ai/ namespace convention

**Status:** Active
**Supersedes:** 0001
**Date:** 2026-06-18
**Revised:** 2026-06-19

## Context

ADR-0001 established `ai/` as a custom namespace for methodology submodules. What was missed was Claude Code ships a formal plugin system (Oct 2025) that solves the same distribution problem — marketplace discovery, namespaced skills, setup lifecycle, version management. The `ai/` convention was a bespoke solution to a problem the ecosystem had already solved. OpenCode is also converging on `.claude/skills/` as a fallback discovery path so this can be made cross-agent if necessary and a path was mapped out to make sure there is no vendor-lock.

The initial approach used a git submodule at `.claude/skills/aiml/` for auto-discovery via the skills-dir trust mechanism. This hit a trust-gate bug in Claude Code: the trust dialog never presents, making the plugin permanently unloadable via that path. This is not fixable due to a load-order bug in claude app and `plugin-update` itself and a mis-documentation at Anthropic. `claude plugin enable` cannot recover from this state. The submodule was removed from the parent project.

## Decision

Distribute the maturity ladder via a local marketplace (`claude plugin marketplace add <path>`). The plugin is installed at user scope. For live development, the cache at `~/.claude/plugins/cache/` is symlinked to the source repository. The `ai/` namespace directory is sunset; the `.claude/skills/aiml` submodule approach is abandoned due to the trust-gate bug.

## Consequences

Skills are namespaced as `/aiml:*` (graduation-suggest, setup). The setup skill handles instruction-file wiring (replacing manual CLAUDE.md editing). Marketplace distribution means the plugin works across all projects without per-repo submodule setup. Consumer projects remove both `ai/maturity-ladder` and any `.claude/skills/aiml` submodule, then install from the marketplace. The `ai/openspec-adr` submodule will follow the same pattern in a separate change.

---

*Decided during convert-maturity-ladder-to-plugin change, 2026-06-18. Revised 2026-06-19 after trust-gate bug discovery.*
