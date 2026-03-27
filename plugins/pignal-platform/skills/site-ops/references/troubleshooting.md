# Troubleshooting Guide

Common issues when operating Pignal sites via MCP, with root causes and resolution steps.

## Site Provisioning Issues

### Site stuck in "provisioning" status

**Symptoms:** `list_my_sites` or `get_site_status` shows `provisioning` for more than 5 minutes.

**Root causes and resolution:**

1. **Custom repo build not completing.** If `get_site_status` shows step `wait-for-build`, the GitHub Actions workflow may have failed or the repo was not properly created from the template.
   - Resolution: The user should check their GitHub repo's Actions tab for build failures. Once fixed, retry `create_site` with the same subdomain -- the cleanup step will handle the previous failed attempt.

2. **Cloudflare API rate limiting.** If stuck at `create-d1` or `deploy-worker`, Cloudflare may be throttling API calls.
   - Resolution: Wait 5 minutes and retry `create_site`. The workflow has built-in retries with exponential backoff, but persistent rate limiting requires manual retry.

3. **GitHub token expired.** If `create_site` fails immediately with a token error, the user's GitHub OAuth token has expired.
   - Resolution: The user must re-authenticate via the hub dashboard (`pignal.net/dashboard`). After re-authentication, retry the operation.

### Site status is "failed"

**Symptoms:** `list_my_sites` shows status `failed`.

**Resolution:** Call `get_site_status` to see the specific step and error message. Then:

- Simply call `create_site` again with the same subdomain. The provisioning workflow always starts with a cleanup step that removes resources from the previous failed run, so retrying is safe and idempotent in terms of resource cleanup.

### Subdomain already taken

**Symptoms:** `create_site` returns an error about the subdomain being unavailable.

**Resolution:** Choose a different subdomain. There is no way to claim an existing subdomain unless it belongs to the same user (in which case the existing site should be deleted first).

## Health Check Failures

### Site shows "unhealthy" status

**Symptoms:** `list_my_sites` shows health status `unhealthy`.

**Root causes:**

1. **Worker crashed or has a runtime error.** The site's `/health` endpoint is returning a non-200 status.
   - Resolution: Try `redeploy_site` to push the latest stable code. If the issue persists, check if recent settings changes could cause rendering errors.

2. **D1 database issues.** The Worker cannot connect to its D1 database.
   - Resolution: `redeploy_site` will re-upload the worker with its D1 binding. This resolves most D1 connectivity issues.

3. **Worker was deleted externally.** If someone deleted the worker directly via the Cloudflare dashboard.
   - Resolution: `redeploy_site` will re-create the namespace script.

### Site shows "unknown" health status

**Symptoms:** Health status is `unknown` rather than `healthy` or `unhealthy`.

**Root cause:** The hub cron has not yet performed a health check on this site. This is normal for newly provisioned sites.

**Resolution:** Wait up to 15 minutes for the next cron cycle. If it persists, the source may not be properly registered. Verify the site is accessible by checking its URL directly.

## Redeployment Issues

### Redeploy fails with "not active"

**Symptoms:** `redeploy_site` returns an error saying the site is not in active status.

**Root cause:** The site is in `provisioning`, `failed`, or `deleting` status.

**Resolution:** Only sites with `active` status can be redeployed. If in `failed` status, retry provisioning with `create_site`. If in `provisioning` or `deleting`, wait for the current operation to complete.

### Redeploy succeeds but site shows old content/behavior

**Symptoms:** `redeploy_site` completes successfully but the site appears unchanged.

**Root causes:**

1. **Browser cache.** The user's browser may be serving cached CSS/JS assets.
   - Resolution: Hard refresh (Ctrl+Shift+R / Cmd+Shift+R) or clear browser cache.

2. **CDN cache.** Cloudflare's edge cache may still serve the old version.
   - Resolution: Wait a few minutes for cache expiration. Pignal uses cache-busted static asset URLs, so CSS/JS updates should propagate immediately. HTML pages may take longer.

3. **No actual code changes.** If the user is redeploying the same version, nothing will change.
   - Resolution: Confirm that a new release or code change actually exists before redeploying.

### GitHub token expired during redeploy

**Symptoms:** `redeploy_site` fails with a GitHub token error.

**Resolution:** The user must re-authenticate via the hub dashboard. After re-authentication, retry `redeploy_site`.

## Content Save Failures

### "keySummary too short" or "keySummary too long"

**Symptoms:** `save_item` or `update_item` returns a validation error about keySummary length.

**Root cause:** The keySummary does not meet the site's configured validation limits. Default limits are typically 20-140 characters, but site owners can customize these.

**Resolution:** Call `get_metadata` to see the current `validation_limits`, then adjust the keySummary to fit within the min/max range.

### "content exceeds maximum length"

**Symptoms:** `save_item` returns an error about content length.

**Root cause:** The content exceeds the configured maximum (typically 10,000 characters, hard ceiling at 50,000).

**Resolution:** Shorten the content or split it into multiple items. Call `get_metadata` to see the exact limit.

### "Type not found" or invalid typeId

**Symptoms:** `save_item` returns an error about an invalid type.

**Root cause:** The `typeId` does not match any type configured on the site. Type IDs are UUIDs, not human-readable names.

**Resolution:** Call `get_metadata` to get the current list of types with their IDs. Use the correct UUID.

### "Workspace not found" or invalid workspaceId

**Symptoms:** `save_item` returns an error about an invalid workspace.

**Root cause:** The `workspaceId` does not match any workspace configured on the site.

**Resolution:** Call `get_metadata` to get the current workspace list. If the desired workspace does not exist, create it first with `create_workspace`.

### Item saves but does not appear on the public site

**Symptoms:** `save_item` succeeds but the item is not visible on the site's public page.

**Root cause:** Items are created as `private` by default. They only appear on the public site after being vouched.

**Resolution:** Call `vouch_item` with `visibility: "vouched"` to publish the item.

### Slug conflict when vouching

**Symptoms:** `vouch_item` with a custom slug returns a conflict error.

**Root cause:** Another item already has that slug. Slugs must be unique per site.

**Resolution:** Choose a different slug, or let the system auto-generate one by omitting the `slug` parameter.

## Permission Issues

### "Unauthorized" or "forbidden" on call_site_tool

**Symptoms:** `call_site_tool` returns a 401 or 403 error.

**Root causes:**

1. **Federation token expired or revoked.** The token linking the hub to the site is no longer valid.
   - Resolution for managed sites: This should not happen under normal circumstances. Try `redeploy_site` which re-establishes bindings. If it persists, delete and re-create the site.
   - Resolution for self-hosted sites: The user needs to re-register their site with the hub.

2. **Site not verified.** The hub has not verified ownership of the site.
   - Resolution for managed sites: Provisioning automatically verifies. If verification was lost, the source record may have been corrupted -- re-create the site.
   - Resolution for self-hosted sites: The user needs to verify via the hub dashboard.

### "No tools available for this site"

**Symptoms:** `get_site_tools` returns an empty list.

**Root causes:**

1. **Site not yet active.** Tools are synced during provisioning (step 9: verify-health). If the site is still provisioning, tools will not be available yet.
   - Resolution: Wait for provisioning to complete.

2. **Tool sync failed.** The `/.well-known/pignal` endpoint on the site did not return tool definitions.
   - Resolution: Wait for the next cron cycle (15 minutes) which will attempt to re-sync tools. Or trigger a `redeploy_site` which includes a tool sync step.

3. **Federation not configured.** For self-hosted sites, the site must have a federation key and be registered with the hub.
   - Resolution: The user needs to register and verify their site via the hub dashboard.

## MCP Connection Issues

### "GitHub token expired" on any operation

**Symptoms:** Various tools return GitHub token expiration errors.

**Root cause:** The user's GitHub OAuth token has expired. Hub MCP uses GitHub OAuth for authentication.

**Resolution:** The user must re-authenticate via the hub dashboard or re-authorize the MCP client. This is not something that can be fixed through MCP tool calls alone.

### Timeout on call_site_tool

**Symptoms:** `call_site_tool` takes a long time and eventually times out.

**Root cause:** The target site's Worker is experiencing high latency or is unresponsive.

**Resolution:**
1. Check the site's health status via `list_my_sites`
2. If unhealthy, try `redeploy_site`
3. If the site is healthy but slow, the operation may be legitimately slow (e.g., listing thousands of items). Try reducing the `limit` parameter.

## General Debugging Approach

When encountering an unexpected error:

1. **Read the error message carefully.** Pignal error messages are descriptive and usually point to the exact issue.
2. **Check site status.** Many issues stem from the site not being in `active` status. Call `list_my_sites` first.
3. **Refresh metadata.** If type/workspace IDs seem wrong, call `get_metadata` to get the current state.
4. **Retry once.** Transient network issues between the hub and site workers can cause one-off failures. A single retry often resolves them.
5. **Redeploy as a reset.** For persistent issues with managed sites, `redeploy_site` re-establishes the worker, its bindings, and its health status. It is the closest thing to "turn it off and on again."
