# MCP Workflow Reference

Complete reference for Pignal MCP tool orchestration, including tool descriptions, input details, sequencing patterns, and error handling.

## Tool Reference

### Hub Platform Tools (7)

These tools operate at the hub level -- they manage sites as a whole, not content within sites.

| Tool | Purpose | Idempotent | Destructive |
|------|---------|------------|-------------|
| `list_my_sites` | List all sites with status, health, item counts | Yes | No |
| `create_site` | Provision a new managed site | No | No |
| `delete_site` | Delete a managed site (irreversible) | No | Yes |
| `redeploy_site` | Push latest code to an active site | No | No |
| `get_site_tools` | Discover tools available on a site | Yes | No |
| `call_site_tool` | Execute a tool on a specific site | Depends on tool | Depends on tool |
| `get_site_status` | Check provisioning/deployment progress | Yes | No |

#### `list_my_sites`

No input required. Returns all sites (managed and self-hosted) with:
- Site ID (used as input for all other tools)
- Slug and URL
- Template type
- Status (`active`, `provisioning`, `failed`, `deleting`)
- Health status (`healthy`, `unhealthy`, `unknown`)
- Public item count

**When to use:** Start of any session, before any site-specific operation, or when you need to find a site's ID.

#### `create_site`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `subdomain` | Yes | 2-30 chars, lowercase alphanumeric + hyphens, becomes `{subdomain}.pignal.net` |
| `template` | No | One of 24 templates (default: `blog`) |
| `custom_repo` | No | Create a GitHub repo for customization (default: `false`) |
| `repo_name` | No | GitHub repo name (only with `custom_repo`, defaults to subdomain) |
| `private_repo` | No | Make the custom repo private (only with `custom_repo`) |

**When to use:** User wants a new site. Always confirm template choice and subdomain before creating.

**After calling:** The site enters `provisioning` status. Poll with `list_my_sites` or `get_site_status` until status is `active` (typically 30-90 seconds). Do not attempt `call_site_tool` until active.

#### `delete_site`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | Site ID from `list_my_sites` |

**When to use:** Only when explicitly requested by the user. This is irreversible -- it deletes the Worker, D1 database, routing, and hub source record.

**Always confirm with the user before calling.** There is no undo.

#### `redeploy_site`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | Site ID (must be in `active` status) |

**When to use:** When the user wants to update their site's code to the latest version. Settings changes do NOT require redeployment.

#### `get_site_tools`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | Site ID |

Returns tool names, descriptions, and full JSON Schema input definitions. **Always call this before using `call_site_tool`** if you have not already retrieved the tools for this site in the current session. Tool schemas contain field-level descriptions that explain what each parameter means in the context of the site's template.

#### `call_site_tool`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | Site ID |
| `tool_name` | Yes | Tool name from `get_site_tools` |
| `input` | No | Input parameters matching the tool's schema |

This is the gateway to all 15+ site-level tools. The hub proxies the call securely via federation tokens.

#### `get_site_status`

| Parameter | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | Site ID |

Returns current status, provisioning step (if in progress), error message (if failed), and site URL. More granular than `list_my_sites` for tracking provisioning/deployment progress.

### Site-Level Tools (via `call_site_tool`)

These are the tools available on each site, accessed through `call_site_tool`. The exact schemas come from `get_site_tools`, but the tool names and purposes are consistent across all templates.

#### Content CRUD

| Tool | Purpose | Key Input Fields |
|------|---------|-----------------|
| `save_item` | Create a new item | `keySummary`, `content`, `typeId`, `workspaceId?`, `tags?` |
| `update_item` | Update an existing item | `itemId`, plus any fields to change |
| `delete_item` | Permanently delete an item | `itemId` |
| `archive_item` | Soft-archive an item | `itemId` |
| `unarchive_item` | Restore an archived item | `itemId` |

**Notes on `save_item`:**
- `id` and `sourceAi` are auto-injected -- never include them
- `keySummary` must respect the configured min/max length limits (typically 20-140 chars)
- `content` must respect the configured max length (typically up to 10,000 chars)
- `tags` is an optional array of lowercase strings
- The item is created as `private` visibility by default

#### Content Discovery

| Tool | Purpose | Key Input Fields |
|------|---------|-----------------|
| `list_items` | List items with filtering | `typeId?`, `workspaceId?`, `isArchived?`, `limit?`, `offset?` |
| `search_items` | Full-text search | `query`, `typeId?`, `workspaceId?`, `limit?` |
| `get_metadata` | Get types, workspaces, settings, guidelines | (none) |

**`get_metadata` is the most important discovery tool.** It returns:
- All types with IDs, names, guidance, and validation actions
- All workspaces with IDs, names, and visibility
- Quality guidelines (keySummary tips, content tips, formatting rules, things to avoid)
- Validation limits (min/max character counts)

#### Visibility Management

| Tool | Purpose | Key Input Fields |
|------|---------|-----------------|
| `vouch_item` | Change item visibility | `itemId`, `visibility` (`private`/`unlisted`/`vouched`), `slug?` |
| `batch_vouch_items` | Bulk visibility changes | `items` (array of `{ itemId, visibility, slug? }`) |
| `validate_item` | Apply validation action | `itemId`, `actionId` |

#### Taxonomy Management

| Tool | Purpose | Key Input Fields |
|------|---------|-----------------|
| `create_type` | Create a content type | `name`, `description?`, `color?`, `icon?`, `guidance?`, `actions?` |
| `update_type` | Update a content type | `typeId`, plus fields to change |
| `delete_type` | Delete a content type | `typeId` |
| `create_workspace` | Create a workspace | `name`, `description?`, `visibility?` |
| `update_workspace` | Update a workspace | `workspaceId`, plus fields to change |
| `delete_workspace` | Delete a workspace | `workspaceId` |

#### Settings

| Tool | Purpose | Key Input Fields |
|------|---------|-----------------|
| `update_settings` | Update site settings | `settings` (array of `{ key, value }`) |

#### Site Actions (Forms)

| Tool | Purpose | Key Input Fields |
|------|---------|-----------------|
| `create_action` | Create a form | `name`, `slug`, `fields`, `description?`, `settings?` |
| `update_action` | Update a form | `actionId`, plus fields to change |
| `list_actions` | List all forms | `status?` |
| `delete_action` | Delete a form | `actionId` |
| `list_submissions` | List form submissions | `actionId?`, `status?`, `limit?`, `offset?` |
| `manage_submission` | Update submission status | `submissionId`, `status` |
| `delete_submission` | Delete a submission | `submissionId` |
| `get_submission_stats` | Get submission statistics | (none) |
| `export_submissions` | Export submissions | `actionId`, `format?` (`json`/`csv`) |

## Common Multi-Step Workflows

### Workflow: Create a New Site and Add Content

```
1. create_site(subdomain: "my-blog", template: "blog")
2. [wait] list_my_sites() until status is "active"
3. get_site_tools(id: "<site-id>")
4. call_site_tool(id, "get_metadata")  -- learn types, workspaces
5. call_site_tool(id, "save_item", { keySummary: "...", content: "...", typeId: "..." })
6. call_site_tool(id, "vouch_item", { itemId: "...", visibility: "vouched" })
```

### Workflow: Bulk Content Import

```
1. call_site_tool(id, "get_metadata")  -- get types and workspaces
2. For each item:
   call_site_tool(id, "save_item", { ... })
3. call_site_tool(id, "list_items", { limit: 50 })  -- verify all items created
4. call_site_tool(id, "batch_vouch_items", { items: [...] })  -- publish all at once
```

### Workflow: Set Up Site Structure

```
1. call_site_tool(id, "get_metadata")  -- see existing types/workspaces
2. call_site_tool(id, "create_type", { name: "...", guidance: { whenToUse: "...", pattern: "...", example: "..." } })
3. call_site_tool(id, "create_workspace", { name: "...", visibility: "public" })
4. call_site_tool(id, "update_settings", { settings: [{ key: "source_title", value: "..." }, { key: "source_description", value: "..." }] })
```

### Workflow: Configure Lead Capture

```
1. call_site_tool(id, "create_action", {
     name: "Contact Form",
     slug: "contact",
     fields: [
       { name: "name", type: "text", label: "Name", required: true },
       { name: "email", type: "email", label: "Email", required: true },
       { name: "message", type: "textarea", label: "Message", required: true }
     ]
   })
2. Embed in content: save_item with content containing {{action:contact}}
3. Monitor: call_site_tool(id, "list_submissions", { actionId: "..." })
```

### Workflow: Site Audit and Cleanup

```
1. call_site_tool(id, "list_items", { limit: 50 })  -- review all items
2. For items needing updates: call_site_tool(id, "update_item", { itemId: "...", ... })
3. For outdated items: call_site_tool(id, "archive_item", { itemId: "..." })
4. call_site_tool(id, "get_metadata")  -- review types and workspaces
5. Remove unused types/workspaces if any
```

## Error Handling Patterns

### Transient Errors

Federation calls (via `call_site_tool`) can fail transiently if the target site is temporarily unreachable. Retry once after a brief pause. If it fails again, check the site's health status.

### Validation Errors

`save_item` and `update_item` will return error messages if content violates quality limits (keySummary too short/long, content too long). Read the error message -- it tells you exactly which constraint was violated. Adjust the content and retry.

### Permission Errors

If `call_site_tool` returns a permission error, the federation token may have expired or been revoked. The user may need to re-register their site with the hub. For managed sites, this should not happen under normal circumstances.

### Provisioning Failures

If `create_site` results in a `failed` status, use `get_site_status` to see which step failed and the error message. Common causes:
- Subdomain already taken
- GitHub token expired (re-authenticate via hub dashboard)
- Temporary Cloudflare API issues (retry `create_site` -- cleanup step handles previous failed resources)

### Tool Not Found

If `call_site_tool` says a tool does not exist, the tool definitions may be out of sync. This can happen after a redeploy that changes the tool manifest. The hub cron syncs tools every 15 minutes, or you can trigger a resync by calling `get_site_tools` (which reads from the hub's cached tool definitions).

## Tool Call Sequencing Best Practices

1. **Always discover before writing.** Call `get_metadata` before `save_item` to ensure you use valid type and workspace IDs.

2. **Batch when possible.** Use `batch_vouch_items` instead of individual `vouch_item` calls when publishing multiple items.

3. **Check status after async operations.** `create_site`, `delete_site`, and `redeploy_site` are asynchronous. Always follow up with `list_my_sites` or `get_site_status`.

4. **Use `list_items` for verification.** After creating or updating items, list them to confirm the changes took effect.

5. **Minimize redundant `get_metadata` calls.** The metadata (types, workspaces, settings) rarely changes within a session. Cache the result mentally and only re-fetch if you have made structural changes (created types, workspaces, or changed settings).

6. **Settings before content.** If you are setting up a new site, configure settings (title, description, theme) and create types/workspaces before adding content. This ensures content is properly categorized from the start.
