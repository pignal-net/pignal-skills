# Contributing to Pignal Skills

Thank you for your interest in contributing domain knowledge skills to the Pignal ecosystem.

## What Makes a Good Pignal Skill

Pignal skills teach AI agents **domain craft knowledge** — the expertise that separates generic content from professional-quality output. They are NOT configuration files or template documentation.

### Skills MUST contain:
- **Domain expertise** — what a seasoned professional in this field knows
- **Workflow guidance** — how to approach work using Pignal's MCP tools
- **Quality heuristics** — what "good" looks like and why
- **Explanations of WHY** — reasoning behind every recommendation

### Skills must NOT contain:
- Template metadata (types, workspaces, field limits) — discovered at runtime via `get_metadata`
- Specific field values or IDs — these change per site
- Hardcoded MCP tool names or parameters — discovered at runtime via `get_site_tools`
- Rigid rules without reasoning — explain WHY, not just WHAT

## Skill Quality Checklist

Before submitting a skill, verify:

- [ ] **Domain knowledge, not config:** Does the skill teach craft knowledge that doesn't change when template config changes?
- [ ] **Explains WHY:** Does every recommendation include reasoning? No unexplained MUSTs or ALWAYSes.
- [ ] **Progressive disclosure:** Is SKILL.md under 500 lines with deep dives in `references/`?
- [ ] **Description triggers correctly:** Does the description follow `[What it does] + [When to use it] + [Key capabilities]` format?
- [ ] **MCP workflow included:** Does the skill teach the discover → plan → execute → validate pattern?
- [ ] **No template metadata:** Are types, workspaces, and limits left to runtime discovery?
- [ ] **Concrete use cases:** Are there 2-3 specific scenarios where this skill helps?
- [ ] **Common mistakes section:** Does the skill warn about anti-patterns with explanations?

## How to Contribute

### Adding a New Plugin

1. **Fork** this repository
2. **Create** your plugin in `plugins/pignal-{name}/`
3. **Follow** the plugin structure:
   ```
   plugins/pignal-{name}/
   ├── .claude-plugin/
   │   └── plugin.json
   ├── agents/
   │   └── {domain}-operator.md   (autonomous operator agent)
   ├── skills/
   │   └── {skill-name}/
   │       ├── SKILL.md
   │       └── references/        (optional)
   ├── hooks/
   │   └── hooks.json             (Tier 1 & 2 only)
   └── commands/
       └── operate.md             (all tiers — scheduled task entry point)
       └── *.md                   (Tier 1 & 2: additional specialized commands)
   ```
4. **Write** your SKILL.md following the template below
5. **Write** your operator agent following the agent template below
6. **Add** your plugin entry to `.claude-plugin/marketplace.json`
7. **Test** locally: `claude --plugin-dir ./plugins/pignal-{name}`
8. **Submit** a pull request

### plugin.json Template

```json
{
  "name": "pignal-{name}",
  "description": "{One-line description}",
  "version": "1.0.0",
  "author": {
    "name": "Your Name",
    "email": "your@email.com"
  },
  "homepage": "https://pignal.net/skills",
  "repository": "https://github.com/pignal-net/pignal-skills",
  "license": "MIT",
  "keywords": ["pignal", "{domain}", "{keyword}"]
}
```

### SKILL.md Template

```yaml
---
name: {skill-name}
description: |
  {[What it does] — domain expertise description}.
  {[When to use it] — trigger phrases and contexts}.
  {[Key capabilities] — specific things the skill enables}.
---

# {Domain Title}

{1-2 paragraphs establishing domain authority and why this matters.}

## Working with Your Pignal Site

Before creating or managing content, discover your site's configuration:

1. Call `get_site_tools` to discover available operations with their exact names and schemas
2. Use the discovered metadata tool (via `call_site_tool`) to learn types, workspaces, limits

Note: Reference site tools by intent (e.g., "the content creation tool", "the validation tool") rather than hardcoded names — actual names are discovered at runtime.

## {Domain Craft Section}

{The heart of the skill — stable domain knowledge with WHY explanations.}

## Workflow

{Step-by-step process using MCP tools with domain knowledge applied.}

## Quality Checklist

{Domain-specific self-assessment before publishing.}

## Common Mistakes

{Anti-patterns with explanations of WHY they're harmful.}
```

### Agent Template

Every template plugin should include an operator agent at `agents/{domain}-operator.md`:

```yaml
---
name: {domain}-operator
description: |
  Autonomous operator for Pignal {Template} sites. Analyzes site state,
  researches what the site needs, creates expert-quality {content-noun}s,
  validates, and publishes end-to-end without human input.
  Use when operating, maintaining, or creating content for a Pignal
  {template} site.
tools: [mcp__*, WebSearch, WebFetch]
skills: [{skill-name}]
maxTurns: 100
---

{System prompt with 6-phase workflow}
```

**Agent Quality Checklist:**

- [ ] **Fully autonomous:** Agent NEVER asks for user input
- [ ] **Three modes:** Handles autonomous operation (scheduled), directed creation, and maintenance
- [ ] **Six phases:** Discover → Research & Decide → Plan → Create → Validate & Publish → Report
- [ ] **Phase 2 is domain-specific:** What does this domain research? (e.g., blogs check trends, shops check catalog gaps)
- [ ] **Phase 4 encodes craft:** Content structure, keySummary patterns, quality criteria specific to the domain
- [ ] **Good/bad examples:** Includes keySummary examples showing what good and bad look like
- [ ] **Error handling:** Retry once, skip with note, always complete with report
- [ ] **Structured report:** Reports what was done, what was published, issues, suggestions for next run

**Agent Design Principles:**

- The primary use case is **scheduled autonomous operation** — agents run on a schedule with no human present
- Don't limit Phase 4 to the current SKILL.md content — research and apply professional-grade domain best practices
- The agent should understand the template's real-world purpose: what does a professional in this domain do?
- Agents work through MCP tools (`mcp__*`) and web research (`WebSearch`, `WebFetch`)
- Hooks in Tier 1 and Tier 2 plugins handle cross-cutting quality gates — template agents don't need their own hooks
- Agent prompts reference site tools by **intent**, not hardcoded names (e.g., "the site's validation tool" instead of a specific tool name) — actual names are discovered at runtime via `get_site_tools`
- Hooks check operation **intent** (e.g., "is this a write/mutation operation?") rather than enumerating specific tool names
- Every Tier 3 plugin should include a `commands/operate.md` as a clean entry point for scheduled tasks

### Improving an Existing Skill

1. Read the existing SKILL.md thoroughly
2. Identify the gap — what domain knowledge is missing or could be improved?
3. Make your changes following the same quality standards
4. Test the updated skill locally
5. Submit a PR explaining what you improved and why

## Testing Your Skill

### Local Testing

```bash
# Test your plugin loads correctly
claude --plugin-dir ./plugins/pignal-{name}

# Verify your skill appears
# Type /pignal-{name}:{skill-name} in Claude Code
```

### Triggering Tests

Verify your skill triggers on the right prompts:

**Should trigger:**
- List 3-5 realistic user prompts that should activate your skill

**Should NOT trigger:**
- List 3-5 prompts that are similar but should activate a different skill

### Functional Test

1. Install your skill and the `pignal-platform` skill
2. Ask Claude to perform a task in your domain
3. Verify Claude discovers site configuration first via `get_site_tools` (doesn't assume tool names or config)
4. Verify domain knowledge is applied to the output
5. Compare output quality with vs without the skill

## Code of Conduct

- Be respectful in PRs and discussions
- Focus on craft quality — we're building expert-level domain knowledge
- Acknowledge that domains evolve — skills should be maintained

## License

All contributions are licensed under MIT. By submitting a PR, you agree to this license.
