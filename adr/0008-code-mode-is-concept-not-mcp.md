# 0008 — Code-mode is a concept (typed API wrapper for any external service), not MCP-specific

**Status:** Active
**Date:** 2026-06-10

## Context

The original ladder described code-mode as wrapping MCP calls into TypeScript. things-osascript-service wraps AppleScript (SQLite reads + JXA writes), not MCP, yet it uses the same pattern. A decision was needed about whether code-mode applies only to MCP or to any external capability.

## Decision

Code-mode is defined as the practice of wrapping any external capability (MCP, AppleScript, REST API, system command sequences) as a typed TypeScript API. The uniform interface — typed TypeScript, callable by AI, user, or code without AI mediation — is the defining characteristic. MCP is just one transport layer that can be wrapped.

## Consequences

Any external capability that repeats as a sequence of actions is a code-mode candidate, not just MCP servers. Services built via code-mode must be treated as opaque APIs — callers never bypass the typed interface to reach the underlying transport directly. The things-osascript-service and slack-mcp-service are both valid code-mode examples despite different transports. Rejected alternative: code-mode as MCP-only (per Cloudflare's original definition).

---

*Extracted from [[ai-maturity-ladder-modularization]], [[ai-maturity-ladder]] (habits file), and `openspec/changes/archive/2026-06-10-distill-maturity-ladder-rules/`, originally decided ~2026-06-10.*
