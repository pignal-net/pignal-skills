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
- Hardcoded MCP tool parameters — discovered at runtime
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

### Adding a New Skill

1. **Fork** this repository
2. **Create** your plugin in `plugins/pignal-{name}/`
3. **Follow** the plugin structure:
   ```
   plugins/pignal-{name}/
   ├── .claude-plugin/
   │   └── plugin.json
   └── skills/
       └── {skill-name}/
           ├── SKILL.md
           └── references/     (optional)
   ```
4. **Write** your SKILL.md following the template below
5. **Add** your plugin entry to `.claude-plugin/marketplace.json`
6. **Test** locally: `claude --plugin-dir ./plugins/pignal-{name}`
7. **Submit** a pull request

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
  "repository": "https://github.com/pignalnet/pignal-skills",
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

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` (via `call_site_tool`) to learn types, workspaces, limits

## {Domain Craft Section}

{The heart of the skill — stable domain knowledge with WHY explanations.}

## Workflow

{Step-by-step process using MCP tools with domain knowledge applied.}

## Quality Checklist

{Domain-specific self-assessment before publishing.}

## Common Mistakes

{Anti-patterns with explanations of WHY they're harmful.}
```

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
3. Verify Claude calls `get_metadata` first (doesn't assume config)
4. Verify domain knowledge is applied to the output
5. Compare output quality with vs without the skill

## Code of Conduct

- Be respectful in PRs and discussions
- Focus on craft quality — we're building expert-level domain knowledge
- Acknowledge that domains evolve — skills should be maintained

## License

All contributions are licensed under MIT. By submitting a PR, you agree to this license.
