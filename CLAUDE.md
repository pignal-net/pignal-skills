# CLAUDE.md -- Development Guide for pignal-skills

## Repository Purpose

This is the Claude Code plugin marketplace for the Pignal website platform. It contains 28 plugins that teach AI agents how to operate Pignal-managed websites.

Four component types work together:

- **Skills** -- Domain craft knowledge that AI agents apply when creating and managing content. Skills encode what a seasoned professional in a given field knows: editorial judgment, quality heuristics, and workflow patterns. They do NOT contain template configuration (types, workspaces, field limits), which is always discovered at runtime.
- **Agents** -- Autonomous workflow orchestrators that run end-to-end without human input: research, decide, execute, publish, report. Designed primarily for scheduled operation in Docker containers.
- **Hooks** -- Quality gates that enforce best practices (e.g., requiring metadata discovery before write operations). All hooks use `type: prompt` because plugins are mounted read-only in agent containers.
- **Commands** -- User-invocable shortcuts (slash commands) for common operations like checking site status or creating a new site.

## Three-Tier Architecture

### Tier 1: Core -- `pignal-platform`

Always installed as a prerequisite. Every other plugin assumes it is co-installed.

Provides:
- `.mcp.json` connecting to the Pignal Hub MCP server (`https://mcp.pignal.net/mcp`)
- `settings.json` with default configuration
- `site-operator` agent for site lifecycle management (provisioning, deployment, monitoring)
- Quality gate hooks for all MCP write operations
- Commands: `site-status`, `new-site`

### Tier 2: Cross-Domain Specialists -- `pignal-seo`, `pignal-content-ops`, `pignal-forms`

Template-agnostic plugins that work on any site regardless of template.

Provides:
- `seo-auditor` agent, `content-reviewer` agent
- Hooks: publish-without-validation warnings, slug quality checks
- Commands for common cross-domain operations
- Skills: SEO mastery, content operations, form management

### Tier 3: Template Operators (24 plugins)

One plugin per Pignal template, each containing a `{domain}-operator` agent.

Plugins: `pignal-awesome`, `pignal-blogger`, `pignal-bookshelf`, `pignal-case-studies`, `pignal-changelog`, `pignal-course`, `pignal-directory`, `pignal-flashcards`, `pignal-glossary`, `pignal-incidents`, `pignal-journal`, `pignal-magazine`, `pignal-menu`, `pignal-podcast`, `pignal-portfolio`, `pignal-recipes`, `pignal-resume`, `pignal-reviews`, `pignal-runbook`, `pignal-services`, `pignal-shop`, `pignal-til`, `pignal-wiki`, `pignal-writing`.

Each operator handles three modes:
1. **Autonomous operation** -- Scheduled runs that independently decide what the site needs
2. **Directed creation** -- User requests specific content
3. **Maintenance** -- Auditing, updating, and improving existing content

## Agent Design Philosophy

**The primary use case is scheduled autonomous operation.** Agents run on schedule via the Local Agent Platform -- Docker containers with `permissionMode: 'dontAsk'` and no human in the loop.

Core principles:

- Agents NEVER ask for user input. They research, decide, execute, and report independently.
- Every agent follows the same 6-phase workflow (the canonical operator pattern).
- Agents produce a structured report at the end of every run.
- Skills provide the domain craft knowledge that agents apply during content creation.
- All template metadata (types, workspaces, field limits) is discovered at runtime, never hardcoded.

Three operating modes:
- **Autonomous** -- Agent decides what to create based on site gaps and domain research
- **Directed** -- Agent receives a topic or brief and creates accordingly
- **Maintenance** -- Agent audits existing content, identifies issues, and fixes them

## The Canonical Operator Pattern

Every Tier 3 agent follows this 6-phase workflow:

### Phase 1: Discover Site State

Call MCP tools to understand the current site:
1. `list_my_sites` -- Find the target site
2. `get_site_tools` -- Learn what tools the site supports
3. `get_metadata` (via `call_site_tool`) -- Learn types, workspaces, tag options, field limits
4. `list_items` (via `call_site_tool`) -- See what content already exists

### Phase 2: Research and Decide (domain-specific)

This phase varies by template. The agent applies domain expertise to decide what the site needs. Examples:
- A blogger-operator might identify missing topic clusters
- A recipes-operator might plan seasonal or technique-based additions
- A glossary-operator might find undefined terms referenced elsewhere

### Phase 3: Plan Each Item

For each piece of content to create, determine:
- Type (from discovered metadata)
- Workspace assignment
- Tags (from available tag vocabulary)
- `keySummary` (concise description for the item)
- Content structure and scope

### Phase 4: Create Content (domain-specific)

Apply domain craft knowledge from skills. This phase is template-specific -- a recipe has different quality standards than a case study. The agent uses skill knowledge to produce professional-quality output.

### Phase 5: Validate and Publish

1. `validate_item` (via `call_site_tool`) -- Check E-E-A-T trust signals and content quality
2. Fix any validation issues
3. `vouch_item` (via `call_site_tool`) -- Move item from `private` to `vouched` (publicly visible) with a descriptive slug

### Phase 6: Report

Produce a structured summary:
- What was created or modified
- Key decisions made and reasoning
- Any issues encountered
- Suggestions for future runs

## Plugin Structure

```
plugins/pignal-{name}/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata (name, description, version, keywords)
├── agents/
│   └── {domain}-operator.md # Autonomous operator agent (Tier 3)
├── hooks/
│   └── hooks.json           # Quality gate hooks (Tier 1 and 2 only)
├── commands/
│   └── *.md                 # User-invocable slash commands (Tier 1 and 2 only)
├── skills/
│   └── {skill-name}/
│       ├── SKILL.md         # Domain craft knowledge (under 500 lines)
│       └── references/      # Deep-dive reference docs for progressive disclosure
├── .mcp.json                # MCP server config (Tier 1 only)
└── settings.json            # Default settings (Tier 1 only)
```

Not every plugin uses every component. Current Tier 3 plugins have skills only; agents, hooks, and commands are being added.

## Key Constraints

### Docker Read-Only Mounts

Plugins are mounted read-only in agent containers. This means:
- Hooks MUST use `type: prompt` (LLM-evaluated), not `type: command` (shell-executed)
- Hooks cannot write to `${CLAUDE_PLUGIN_ROOT}` or any plugin directory
- All state lives in the Pignal platform (via MCP), not in local files

### Cross-Plugin Dependency

`pignal-platform` provides `.mcp.json` which configures the Pignal Hub MCP server connection. All other plugins assume this is available. Never duplicate MCP configuration in Tier 2 or Tier 3 plugins.

### Hook Matcher Patterns

Use `mcp__.*__tool_name` regex patterns for hook matchers. This ensures portability across environments where the MCP server name may vary (Claude Code CLI, Claude.ai, Agent SDK). Example:

```json
{
  "matcher": "mcp__.*__call_site_tool",
  "type": "prompt"
}
```

### Skill Content Rules

Skills contain domain craft knowledge, not template configuration:
- NEVER hardcode types, workspaces, or field limits -- these are discovered via `get_metadata`
- ALWAYS explain WHY behind every recommendation, not just WHAT
- Keep SKILL.md under 500 lines; put deep dives in `references/`
- Skill descriptions follow: `[What it does] + [When to use it] + [Key capabilities]`

## MCP Orchestration Pattern

Every Pignal operation follows: **discover, plan, execute, validate**.

Key MCP tools (all accessed through the Pignal Hub server):

| Tool | Purpose |
|------|---------|
| `list_my_sites` | Find sites owned by the current user |
| `get_site_tools` | Discover tools available on a specific site |
| `call_site_tool` | Execute any site-specific tool (save, list, validate, vouch, etc.) |
| `get_site_status` | Check provisioning and deployment status |
| `create_site` | Provision a new site |
| `redeploy_site` | Deploy latest code |

Content lifecycle:
1. Content starts as `private` (draft)
2. `validate_item` checks E-E-A-T trust signals before publishing
3. `vouch_item` moves content to `vouched` (publicly visible) with a descriptive slug
4. Always call `get_metadata` before any write operation to learn current site configuration

## Testing

### Test a Single Plugin

```bash
claude --plugin-dir ./plugins/pignal-platform
```

### Test Multiple Plugins Together

```bash
claude --plugin-dir ./plugins/pignal-platform \
       --plugin-dir ./plugins/pignal-blogger \
       --plugin-dir ./plugins/pignal-seo
```

### Reload After Changes

Inside a Claude Code session, run `/reload-plugins` to pick up changes without restarting.

### Functional Verification

1. Install your plugin alongside `pignal-platform`
2. Ask Claude to perform a task in your domain
3. Verify Claude calls `get_metadata` first (does not assume configuration)
4. Verify domain knowledge from skills is applied to output
5. Compare output quality with and without the skill installed

## How to Add a New Template Operator

1. **Create the agent file:** `plugins/pignal-{name}/agents/{domain}-operator.md`
2. **Start from the canonical operator pattern** -- all 6 phases must be present
3. **Understand the template's real-world purpose.** What does a professional in this domain do? A recipe author thinks differently than a changelog maintainer.
4. **Write Phase 2 (Research and Decide)** specific to the domain. What research does this operator need to decide what content to create?
5. **Write Phase 4 (Create)** specific to the domain. What are the content structure, quality standards, and craft techniques for this type of content?
6. **Ensure the agent never asks for input.** In autonomous mode, it must research, decide, and act independently.
7. **Test locally:**
   ```bash
   claude --plugin-dir ./plugins/pignal-platform \
          --plugin-dir ./plugins/pignal-{name}
   ```

## Common Development Tasks

### Modifying a Skill

1. Read the existing SKILL.md and its references
2. Identify the gap in domain knowledge
3. Make changes following skill content rules (craft knowledge, not config)
4. Test that the skill triggers on appropriate prompts
5. Verify SKILL.md stays under 500 lines

### Adding a Hook

1. Hooks go in `plugins/pignal-{name}/hooks/hooks.json`
2. Use `type: prompt` (not `type: command`) for Docker compatibility
3. Use `mcp__.*__tool_name` matcher patterns for environment portability
4. Test that the hook fires on the intended MCP tool calls

### Adding a Command

1. Commands go in `plugins/pignal-{name}/commands/{command-name}.md`
2. Commands are user-invocable via `/plugin-name:command-name`
3. Keep commands focused on a single operation
4. Commands are primarily for Tier 1 and Tier 2 plugins
