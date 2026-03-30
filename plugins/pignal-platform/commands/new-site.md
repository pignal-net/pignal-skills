---
description: Create a new Pignal site with full autonomous setup
argument-hint: "[subdomain] [template]"
allowed-tools: [mcp__*, WebSearch, WebFetch]
---

Parse the arguments to extract subdomain and template.
If arguments are missing, ask the user.
Then execute the full site creation workflow:
1. create_site with the specified subdomain and template
2. Poll get_site_status until active
3. Configure settings, create workspaces, seed initial content
4. Report the result with site URL
