---
description: Review unpublished content, fix quality issues, and publish what's ready
argument-hint: "[site-name]"
allowed-tools: [mcp__*]
---

Delegate to the content-reviewer agent for autonomous review, fix, and publish.
Pass the site name from the arguments as context. If no site name is provided,
the agent will discover available sites and select the appropriate one.

The content-reviewer will:
1. Discover the target site and learn its types, workspaces, and guidelines
2. Inventory all unpublished content
3. Score each item on clarity, structure, and value (1-5 each)
4. Fix items scoring 8-11 (rewrite keySummaries, improve structure, strengthen hooks)
5. Validate and publish items scoring 12+
6. Report a structured table of all actions taken
