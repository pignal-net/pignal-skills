---
description: Run an SEO audit on a Pignal site and fix issues automatically
argument-hint: "[site-name]"
allowed-tools: [mcp__*, WebSearch]
---

Delegate to the seo-auditor agent for autonomous SEO audit and remediation.
Pass the site name from the arguments as context. If no site name is provided,
the agent will discover available sites and select the appropriate one.

The seo-auditor will:
1. Discover the target site and learn its types, workspaces, and guidelines
2. Inventory all content (published and private)
3. Audit each item across 6 dimensions (keySummary, tags, structure, validation, slug, freshness)
4. Fix what can be fixed (rewrite keySummaries, normalize tags, add validation)
5. Report quick wins fixed, opportunities for human action, and site-level recommendations
