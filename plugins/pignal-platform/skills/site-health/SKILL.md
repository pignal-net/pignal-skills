---
name: site-health
description: |
  Pignal site health monitoring and diagnostics. Use when checking site status, troubleshooting issues, verifying deployments, or monitoring site health.
allowed-tools: [mcp__*, Read, Grep, Glob, WebFetch]
---

# Site Health & Monitoring

Monitor, diagnose, and maintain the health of Pignal sites. Use this skill whenever checking site status, interpreting health checks, diagnosing availability or performance issues, understanding monitoring data, or deciding whether a site needs intervention. This skill covers health check interpretation, diagnostic workflows, proactive monitoring patterns, and escalation decisions.

## How Health Monitoring Works

Pignal's hub continuously monitors all registered sites (both managed and self-hosted) via an automated cron system that runs every 15 minutes. Understanding this system helps you interpret health data correctly and know what actions are useful versus redundant.

### The Health Check Pipeline

Every 15 minutes, the hub-cron worker dispatches a `SourceSyncWorkflow` for each registered site. Each workflow instance is durable (survives worker restarts) and runs 5 sequential steps:

1. **Health check** -- HTTP GET to `/health`. A 200 response means the Worker is running and can connect to its D1 database.
2. **Fetch well-known** -- HTTP GET to `/.well-known/pignal`. Returns site metadata, stats (public item count), owner info, and tool definitions.
3. **Sync tools** -- Updates the hub's `source_tools` table with the latest tool definitions from the site. This is how the hub knows what tools are available via `get_site_tools`.
4. **Update health status** -- Writes `healthy` or `unhealthy` to the source record in hub D1.
5. **Record health history** -- Appends a timestamped health record for historical tracking.

**Key insight:** Health checks are pure HTTP calls to the site's public endpoints. They do not require authentication and do not consume the site's API quota. A `healthy` status means the site responded successfully within the last 15 minutes.

### Health Status Values

| Status | Meaning | Typical Cause |
|--------|---------|---------------|
| `healthy` | Site responded 200 to `/health` within the last 15 minutes | Normal operation |
| `unhealthy` | Site returned non-200 or did not respond | Worker error, D1 issue, or network problem |
| `unknown` | No health check has been performed yet | Newly provisioned site, or site was just registered |

## Checking Site Health

### Quick Health Check

```
list_my_sites
```

This returns health status for all sites at a glance. Look at the `Health:` field for each site.

### Detailed Status Check

For a specific site, especially during or after provisioning:

```
get_site_status(id: "<site-id>")
```

This returns the deployment status, current provisioning step (if applicable), and any error messages. It is more granular than `list_my_sites` for diagnosing provisioning or redeployment issues.

## Interpreting Health Data

### Healthy Sites

A `healthy` status with a reasonable public item count means the site is operating normally. No action needed.

**What "healthy" tells you:**
- The Cloudflare Worker is running
- The Worker can connect to its D1 database
- The site can serve the `/.well-known/pignal` endpoint (which exercises the full stack)
- Tool definitions are synced and up to date

**What "healthy" does NOT tell you:**
- Whether the content is correct or up to date
- Whether the site's theme/settings are configured as desired
- Whether all items are properly categorized
- Whether the site performs well under load (health checks are single requests)

### Unhealthy Sites

An `unhealthy` status means the most recent health check failed. This could be transient or persistent.

**Before taking action, consider:**

1. **Is it transient?** A single failed health check does not necessarily mean a real problem. Cloudflare Workers can occasionally experience brief connectivity issues. Wait for the next health check cycle (15 minutes) and check again.

2. **Was there a recent change?** If the user just redeployed or changed settings, the site may be in a brief transition state.

3. **Is the site accessible directly?** Ask the user to try visiting the site URL in a browser. If it loads, the health check failure was likely transient.

### Unknown Status

An `unknown` status simply means no health check has been recorded. This is expected for:
- Sites provisioned within the last 15 minutes
- Sites that were just registered with the hub
- Sites where the cron worker has not run since registration

**Resolution:** Wait up to 15 minutes. If it persists beyond 30 minutes, there may be an issue with the site's registration or the cron worker.

## Diagnostic Workflows

### Workflow: Site Appears Down

When a user reports their site is not working:

```
1. list_my_sites  -- check status and health
2. If status is not "active": check get_site_status for provisioning/deployment state
3. If status is "active" but health is "unhealthy":
   a. Try redeploy_site to push fresh code
   b. Wait 1-2 minutes
   c. Check list_my_sites again
4. If health is still "unhealthy" after redeploy: escalate (see below)
```

### Workflow: Content Not Appearing on Public Site

When items exist but do not show on the public page:

```
1. call_site_tool(id, "list_items")  -- confirm items exist
2. Check visibility: items must be "vouched" to appear publicly
3. If items are private: call_site_tool(id, "vouch_item", { itemId, visibility: "vouched" })
4. If items are vouched but still not showing:
   a. Check site health via list_my_sites
   b. If unhealthy: redeploy_site
   c. If healthy: the issue may be browser caching -- advise hard refresh
```

### Workflow: Tools Not Available

When `get_site_tools` returns empty or missing expected tools:

```
1. list_my_sites  -- confirm site is "active"
2. If active: tools may not have synced yet (wait up to 15 minutes)
3. If still empty after 15 minutes: redeploy_site (includes tool sync step)
4. After redeploy: get_site_tools again
5. If still empty: the site's /.well-known/pignal endpoint may be broken -- escalate
```

### Workflow: Post-Redeploy Verification

After a `redeploy_site` operation:

```
1. get_site_status(id)  -- watch for status to return to "active"
2. list_my_sites  -- verify health is "healthy"
3. get_site_tools(id)  -- confirm tools are synced
4. call_site_tool(id, "list_items", { limit: 5 })  -- verify data is intact
```

Redeployment preserves all D1 data (items, types, workspaces, settings). It only updates the Worker code and its bindings. Data loss from redeployment is not expected.

## Proactive Monitoring Practices

### Before Adding Content

Before starting a content creation session, verify the target site is healthy:

```
list_my_sites
```

If the site is unhealthy, resolve the issue first. Adding content to an unhealthy site may result in federation failures that lose your work (the hub cannot proxy tool calls to an unreachable site).

### After Bulk Operations

After creating, updating, or deleting many items, verify the site is still healthy:

```
list_my_sites
```

Bulk operations are proxied through federation tokens and executed sequentially. If the site became unhealthy mid-operation, some operations may have failed silently.

### After Settings Changes

Settings changes take effect immediately at runtime and do not require redeployment. However, certain settings (like custom CSS or theme colors) affect page rendering. If the site becomes unhealthy after a settings change, it may be due to malformed custom CSS that causes a rendering error.

**Resolution:** Use `update_settings` to revert the problematic setting, or set it to an empty string.

## When to Escalate vs Self-Fix

### Self-fixable issues (try these first)

| Issue | Fix |
|-------|-----|
| Unhealthy after 1 check | Wait 15 minutes for next check |
| Unhealthy persistently | `redeploy_site` |
| Tools not synced | Wait 15 minutes, or `redeploy_site` |
| Items not showing publicly | `vouch_item` to set visibility |
| Settings not applied | Verify via `get_metadata`, re-apply if needed |
| Provisioning failed | Retry `create_site` with same subdomain |

### Requires user action (cannot fix via MCP)

| Issue | User Action |
|-------|-------------|
| GitHub token expired | Re-authenticate via hub dashboard |
| Custom repo build failing | Fix build issues in their GitHub repo |
| Self-hosted site unreachable | Check their Cloudflare Workers deployment |
| Want custom domain | Not yet supported -- use subdomain |

### Requires platform support (rare)

| Issue | Indicators |
|-------|------------|
| D1 database corruption | Items return inconsistent data after redeploy |
| Dispatch routing broken | Site URL returns 404 despite active status |
| Cron worker not running | No health checks recorded for any site for 30+ minutes |
| Federation token crypto failure | Permission errors on managed sites that persist after redeploy |

These issues are platform-level problems that cannot be resolved through MCP operations. Advise the user to report the issue through the hub dashboard or GitHub issues.

## Understanding the Monitoring Timeline

A typical healthy site's monitoring timeline:

```
T+0:00  -- Cron triggers SourceSyncWorkflow
T+0:02  -- Health check passes (200 from /health)
T+0:03  -- Well-known fetched, tools synced
T+0:04  -- Health status updated to "healthy" in hub D1
T+0:05  -- Health history recorded
...
T+15:00 -- Next cron cycle begins
```

If a site goes down at T+5:00, it will not be detected until T+15:00 (next health check). The maximum detection delay is ~15 minutes. After detection, the status changes to `unhealthy` and the hub dashboard will reflect this.

There is no alerting system -- health status is passive and must be checked. If the user needs to know about downtime immediately, they should check `list_my_sites` periodically or set up external monitoring (e.g., Uptime Robot) pointing at their site's `/health` endpoint.
