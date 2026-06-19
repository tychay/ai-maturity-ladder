# The AI Maturity Ladder for Automation

With the increased use of AI, this framework addresses how to:
1. Make it work to improve **productivity**;
2. Avoid the temptation of laziness and making the AI think for you; and
3. Keep on a growth path of **deliberate practice**.

These three goals form an Inconsistent Triad for most AI users:
1. **Productivity**: AI productivity gains help with performance, not practice.
2. **Avoid laziness**: It is easier to dictate the desired outcome than to think thoroughly about each step needed to get there.
3. **Deliberate practice**: But those outcomes are bereft of the feedback necessary to make this a practice, not a performance.

The purpose of this Ladder is to **use** the AI to _graduate work away from the AI_. By directing the AI to follow this Ladder, productivity becomes a practice which keeps creativity and improvement always a part of the cycle.

## TLDR

If everything that can be automated without an AI is done without an AI, fewer tokens are used per operation. If fewer tokens are spent, then the context window is focused on the problem itself.

The larger the context window, the more "ill-posed" the problem becomes. In mathematics/physics, an **ill-posed problem** has many solutions with no way to pick the right one without additional constraints. More tokens in context → more degrees of freedom for the model → more ways to be wrong → more hallucinations and unintended side-effects. Reducing tokens in the context window is not about cost, it's about _precision_.

The automations should be built like good software architecture (e.g., [Robert Martin's Principles of Package Design](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)), grown organically through the Ladder.

The key insight from [Code Mode](https://blog.cloudflare.com/code-mode/) is that the AI can write software to climb the Maturity Ladder.

## The Ladder (from the top down)

1. The AI starts doing things directly: MCP calls, reading files, etc. This solves the task at hand.
2. As the pattern repeats, it should be built into a **skill**. Since skills are almost markdown descriptions of the direction provided in the chat, this begins to codify the process (and narrow the context window).
3. If repetition happens (e.g., the same set of actions done by two skills), the skill can be broken down into **sub-skills** referenced from both.
4. Next, the work of a skill or sub-skill can be split using the [Workflow Automation Framework](#the-workflow-automation-framework-waf-and-the-ladder). This turns the automation into a workflow controlling a set of tools.
5. The individual steps in the workflow and tools can be broken down into **sub-workflows and sub-tools** chained together.
6. A level can be reached where the sub-tool or sub-workflow can be replaced by **deterministic software**. The AI should build that software into a tool and call it. The token cost is minimal — a single tool with a single responsibility.
7. If a sub-workflow is composed entirely of tool calls, it can often be replaced by deterministic software. The AI should build that sub-workflow into a tool. If it [still needs AI as an agent](#ai-justification-test), the deterministic software should be built into a workflow that calls AI only where justified.

What would be left once the ladder is complete (climbed top-down) is:
1. A chain of fully automated tools (the skill is a thin wrapper); or
2. A workflow with all deterministic parts in tools, where the AI's responsibility is only the reasoning and synthesis that only an AI can do.

No matter the case, the usage is efficient in terms of tokens and intent, and the workflow solves the task in the most well-posed manner possible.

## The Workflow Automation Framework (WAF) and the Ladder

### WAF Defined

WAF in this context means traditional workflow automation (IFTTT, Zapier, etc.): Triggers (the when), Data Mapping (the connections), Conditions (the logic), and Actions (the do).

One way of looking at the Ladder (top-down) is to push all parts down and make clear which need "AI-as-reasoning" vs. "code-as-execution". In traditional WAF, all steps are deterministic code. In the Ladder, it starts with AI doing everything, and we graduate each step to code _unless_ the AI is justified (see below).

### About AI WAF

In AI there is a concept of "The AI Workflow Automation Framework" with 4 steps: Ingest → Analyze → Decide → Execute. This is **not** what this Ladder means. That model assumes AI is the orchestrator at every step. This ladder achieves the opposite: the AI is either removed entirely, or present only in parts where _reasoning over unstructured data_ cannot be replaced.

In AI WAF, the workflow **is** the AI. In this maturity ladder, the workflow is ideally code where AI is a specialist tool for specific parts.

### AI Justification Test

AI is justified in a workflow step only when the step requires reasoning over natural language or unstructured data:

1. **Structuring** (data mapping input): unstructured input → structured output (e.g., parsing free text into fields, extracting entities, classifying)
2. **Composing** (data mapping output): structured input → unstructured output (e.g., summarizing data into prose, drafting a message)
3. **Judging** (condition): a condition that requires semantic understanding (e.g., sentiment, relevance, quality, intent)

Anything else can eventually be pure code. If the AI is doing it, that's a "code smell" — the step hasn't been graduated up the Ladder yet.

### Triggers

Triggers are not part of the Ladder. They are about "what starts a workflow," not "what does work." The Ladder is focused only on the latter.

Once a workflow exists at any level, it can be triggered by:
- **Manual**: user runs a command or invokes a skill
- **Hook**: Claude Code fires a shell command based on an event
- **Scheduled**: a cron job or equivalent runs the workflow on a timer
- **Chained**: another workflow triggers it as a sub-step

**There should never be AI reasoning in the trigger.** If a trigger needs AI reasoning to decide whether to fire, it's actually a condition masquerading as a trigger — split it.

### Actions

Actions should be deterministic code. If they are not, the WAF decomposition needs to be applied (a "code smell"). The Ladder (from the bottom-up) explains the mechanism to get there.

## The Ladder (from the bottom-up)

The best way to see this is with MCP and the [code-mode](https://blog.cloudflare.com/code-mode/) progression.

Whenever the AI does an MCP tool call, this is a "code smell" that it can (and should) be automated. The path: **always wrap MCP calls** as typed TypeScript wrappers even for 1:1 calls. This is the first step to enable automation without the AI in the loop.

When work chains or operates on MCP calls but can be automated, that's an opportunity to write a **separate tool** that orchestrates multiple tools. In WAF-speak: these are deterministic conditions/data mappings or actions.

This extends to APIs/RPCs that are not MCP. They can be wrapped the same way: typed TypeScript wrappers presenting the same interface for tool-chaining. Code-mode is not a prescription tied to MCP — it's a concept saying that any action handling an external capability can be turned into a typed API (deterministic code) callable without AI mediation. It encapsulates RPC actions grouped by a suite ("service") into a uniform interface callable by AI, user, or software.

This chain becomes a workflow/sub-workflow that is fully automated, stored in the tools folder — a standalone tool with no AI necessary in the loop.

## Prescriptions

### How to graduate (top-down)

- If any pattern repeats, suggest graduating it: act → skill → chained skill → workflow automation framework
- Inside a workflow or skill, patterns suggest graduating into sub-workflows or chained skills
- Once mapped as a WAF, there is a clear model of what can be deterministic vs. what must have AI involvement. Split to reduce AI to a sub-agent inside the WAF.
- Track token costs as a proxy for graduation priority

### How to graduate (bottom-up)

- Every MCP call is an opportunity to write a code-mode wrapper. Suggest turning repeated MCP calls into a code-mode API.
- Any action done outside the project is an opportunity for a code-mode wrapper if it can be logically turned into a service.

### Graduation path for triggers

Triggers have a simpler, separate path:
1. **Manual** — user types a command or asks AI to perform a skill
2. **Hook** — Claude Code fires it automatically on an event
3. **System trigger** — cron, filesystem watcher, git hook, webhook, etc. (fully decoupled from Claude Code)

### How to write tools

When writing any tools, **use spec-driven development via OpenSpec.** This forces good package principles and shows opportunities to decompose into sub-tools.

## Examples

These are early attempts before the Ladder was formalized. Each illustrates different levels.

### `import-ulysses` — Level 7 (standalone tool)

A tool importing a Ulysses database into Obsidian markdown. The entire workflow is deterministic (read XML → transform → write files). Built with OpenSpec, making debugging tractable when bugs required re-running. Demonstrates why deterministic tools beat ad-hoc AI actions for reproducibility.

### `things-sync` — Level 6-7 (workflow tool)

A fully deterministic workflow orchestrating `things-osascript-service` (a code-mode wrapper for Things via sqlite reads and URI writes) with file operations on an Obsidian vault. Demonstrates code-mode as a concept beyond MCP — the service abstracts the implementation details away from the workflow.

### `slack-mcp-service` — bottom-up code-mode wrapper

A typed TypeScript wrapper over Slack MCP operations. Enables future workflows to call Slack without AI mediating each operation. AI's role shrinks to orchestration and data transforms that pass the AI Justification Test.

### `/start-day` — Level 2 (skill)

A codified skill replacing a repeated ad-hoc pattern. Graduation path: the mechanical parts (file operations) become a tool; what remains is AI scanning notes for "what's noteworthy" (unstructured→structured).

### `/end-day` → `/update-accomplished` — Level 3 (skill decomposition)

One skill calling another as a sub-skill, avoiding logic duplication. Demonstrates organic refactoring through the Ladder — following package principles without premature architecture.

### `import-day-one` / `import-taskpaper` — Level 7 (standalone tools)

Same shape as `import-ulysses`: read proprietary format → transform to Obsidian markdown → write files. Three similar tools — not yet a framework because the roles differ (checklists vs. templates vs. metadata). Three similar things is better than a premature abstraction.
