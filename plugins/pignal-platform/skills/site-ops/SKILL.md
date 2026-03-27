# Site Operations

Manage the full lifecycle of Pignal sites -- creation, content management, configuration, and deployment -- through MCP. Use this skill whenever working with Pignal sites: creating new sites, saving or organizing content, managing types and workspaces, configuring settings, redeploying, or deleting sites. This skill covers the MCP orchestration pattern, template selection, content workflows, visibility strategy, and site configuration principles.

## The MCP Orchestration Pattern

Every Pignal operation follows a four-phase pattern: **discover, plan, execute, validate**. This pattern exists because Pignal sites are dynamic -- each site has its own types, workspaces, validation rules, and quality guidelines configured by the owner. Hardcoding assumptions about a site's structure leads to malformed content that fails validation.

### Phase 1: Discover

Always start by understanding what you are working with.

**For a new session or unfamiliar site:**

1. `list_my_sites` -- see all sites, their status, health, and item counts
2. `get_site_tools` with the target site's `id` -- learn what tools are available and their exact input schemas
3. `call_site_tool` with `get_metadata` -- retrieve types, workspaces, quality guidelines, and validation limits

**Why this matters:** The `get_metadata` response contains template-specific vocabulary (a blog calls items "articles" while a shop calls them "products"), keySummary length constraints, content formatting rules, type-specific guidance patterns, and workspace organization. Skipping discovery means guessing at type IDs, missing required fields, or violating quality guidelines.

### Phase 2: Plan

Before executing write operations, decide:

- Which **type** best fits the content (use the `whenToUse` guidance from metadata)
- Which **workspace** organizes it (or whether a new workspace is needed)
- What **visibility** level is appropriate
- Whether the keySummary follows the type's `pattern` (e.g., "How to..." for tutorials)
- Whether content respects the configured length limits

### Phase 3: Execute

Call the appropriate tools in the right sequence. See the [MCP workflow reference](references/mcp-workflow.md) for common multi-step patterns.

### Phase 4: Validate

After writes, confirm the result:

- `list_items` to verify the item appears with correct type and workspace
- Check that the keySummary and content meet the quality guidelines
- For visibility changes, verify the item slug was generated (for vouched items)

## Choosing the Right Template

Pignal has 24 templates, each designed for a specific content domain and layout pattern. The template determines the site's public appearance, vocabulary, and default types/workspaces -- but all templates share the same underlying tools and data model.

### Template Selection Framework

Choose based on two axes: **content type** and **presentation style**.

**Content-first sites** (long-form, article-oriented):
- `blog` -- General-purpose feed with type badges and reading time. The default choice when nothing else fits.
- `writing` -- Distraction-free reading with large typography. Choose when the content IS the product (fiction, essays, poetry).
- `magazine` -- Featured hero article with mixed-size cards. Choose for editorial content with visual hierarchy.
- `wiki` -- Tree-structured navigation. Choose when content is reference material meant to be browsed non-chronologically.
- `course` -- Sequential lessons with chapter navigation. Choose when order matters and learners progress through material.
- `runbook` -- Step-by-step procedures. Choose for operational documentation with prerequisites and system grouping.

**Entry-focused sites** (short-form, micro-content):
- `til` -- Ultra-compact daily snippets. Choose for brief learnings and quick notes.
- `journal` -- Date-first personal entries. Choose for private-first reflective writing.
- `flashcards` -- Question/answer grid. Choose for study material with spaced-repetition actions.
- `glossary` -- Searchable alphabetical table. Choose for term definitions and reference lookups.
- `awesome-list` -- Categorized links. Choose for curated resource lists (GitHub awesome-list style).

**Catalog/listing sites** (structured items with metadata):
- `shop` -- Sidebar categories with product grid. Choose for anything with categories and visual browsing.
- `recipes` -- Grid with prep time, servings, difficulty. Choose for any structured how-to with metadata.
- `bookshelf` -- Book-cover grid with reading status. Choose for media collections (books, movies, games).
- `reviews` -- Star ratings with pros/cons. Choose when comparative evaluation is the primary content.
- `menu` -- Table layout with section pricing. Choose for itemized lists with prices.
- `services` -- Service tiers with availability. Choose for service offerings with deliverables.
- `directory` -- External resource cards. Choose for curated links to external sites.

**Professional/operational sites:**
- `portfolio` -- Visual-dominant grid. Choose for showcasing creative work.
- `resume` -- Single-page structured profile. Choose for personal branding with experience timeline.
- `changelog` -- Version-grouped timeline. Choose for product release notes.
- `incidents` -- Severity-colored timeline. Choose for incident tracking and post-mortems.
- `case-studies` -- Outcome metrics with before/after structure. Choose for client success stories.
- `podcast` -- Episode feed with duration and show notes. Choose for audio/video series.

### When in Doubt

If the user's need does not clearly match a specialized template, `blog` is the safest default. It handles diverse content types well and the feed layout works for most use cases. Ask the user about their primary content type and how visitors will navigate (chronological feed vs. categorical browsing vs. reference lookup) to narrow down the choice.

## Managed vs Self-Hosted

**Managed sites** (hosted on pignal.net):
- Created via `create_site`, provisioned automatically
- Subdomain at `{name}.pignal.net`
- No server management, automatic health monitoring
- Hub handles federation, tool sync, and analytics
- Limitations: no custom domain (yet), no MCP endpoint on the site itself (MCP goes through the hub)

**Self-hosted sites:**
- Deployed by the user to their own Cloudflare Workers account
- Full control: custom domain, direct MCP endpoint, custom templates
- User manages their own D1, secrets, and deployments
- Registered with the hub for directory listing and health monitoring

Both types are managed through the same hub MCP tools. The hub abstracts the differences -- `call_site_tool` works identically for both.

## Site Provisioning

When `create_site` is called, a multi-step provisioning workflow runs asynchronously:

1. **Cleanup** -- removes any leftover resources from a previous failed attempt
2. **Create repo** (custom repos only) -- generates a GitHub repository from the template
3. **Build** (custom repos only) -- waits for the GitHub Actions build to complete
4. **Create D1** -- provisions a fresh Cloudflare D1 database
5. **Deploy worker** -- uploads the bundled code with D1 binding and SERVER_TOKEN
6. **Set routing** -- configures KV-based hostname routing (`subdomain.pignal.net`)
7. **Register source** -- creates the hub source record with health tracking
8. **Create federation** -- generates API keys and federation tokens for hub-to-site communication
9. **Verify health** -- confirms the site is responding and syncs tool definitions
10. **Finalize** -- marks the deployment as active

**Typical timing:** 30-90 seconds for standard provisioning (pre-built release), 2-5 minutes for custom repos (includes GitHub build).

**After calling `create_site`:** Use `list_my_sites` to check when status changes to `active`, or use `get_site_status` for more granular progress (shows the current provisioning step). Do not attempt to call site tools until the site is active.

## Content Management Workflows

### Creating Content

The standard content creation flow:

1. `get_metadata` to learn types, workspaces, and guidelines
2. `save_item` with `keySummary`, `content`, `typeId`, and optionally `workspaceId` and `tags`
3. The item is created as `private` by default
4. Review and validate if needed via `validate_item`
5. Publish via `vouch_item` when ready

**Important behaviors of `save_item`:**
- `id` and `sourceAi` are auto-injected by the hub MCP agent -- do not include them in the input
- `tags` is an array of lowercase strings -- they are normalized (lowercase, deduped, sorted) automatically
- Content supports markdown with content directives (`{{action:slug}}`, `{{cta:...}}`, `{{testimonials}}`)

### Visibility Strategy

Pignal uses a three-tier visibility model. The reason for this progression is SEO: once an item is vouched (public), it gets a permanent URL slug and is indexed by search engines. Changing slugs breaks links and SEO equity.

- **`private`** (default) -- Only accessible via API/MCP. Use during drafting and review.
- **`unlisted`** -- Accessible via a share token URL (`/s/:token`). Use for pre-publication review or content that should be accessible but not discoverable.
- **`vouched`** -- Public, listed on the site, SEO-indexed with a permanent slug. Use for finalized content.

**To publish content:** Call `vouch_item` with `visibility: "vouched"`. You can optionally provide a custom `slug` -- if omitted, one is auto-generated from the keySummary. Slugs must be unique per site.

**Batch publishing:** Use `batch_vouch_items` to publish multiple items at once. This is more efficient than individual `vouch_item` calls and provides a summary of successes and failures.

### Updating Content

Use `update_item` with the `itemId` and only the fields you want to change. You do not need to send unchanged fields. Updatable fields: `keySummary`, `content`, `typeId`, `workspaceId`, `tags`.

### Archiving vs Deleting

- **Archive** (`archive_item`) -- Soft-removes the item. It no longer appears in listings but can be restored with `unarchive_item`. Use for content that is outdated but might be needed later.
- **Delete** (`delete_item`) -- Permanent removal. Use only when the content should never be recovered.

Prefer archiving over deleting. Archived items retain their data and can be restored without re-creation.

## Type and Workspace Management

### Types

Types categorize content within a site. Each template comes with pre-configured types that match its domain (e.g., a blog has "Article", "Tutorial", "Resource"; a shop has "Product", "Service", "Bundle").

**When to create new types:** Only when the existing types do not adequately categorize a piece of content. Types should represent fundamentally different kinds of content, not just topics. If in doubt, use tags for topical organization instead.

**Type guidance:** When creating types via `create_type`, provide `guidance` with `whenToUse`, `pattern` (for keySummary format), `example`, and `contentHints`. These are surfaced to AI clients via `get_metadata` and dramatically improve content quality.

### Workspaces

Workspaces group content into named collections. They can be `public` (shown on the site with filter options) or `private` (organizational only, not visible to visitors).

**When to create workspaces:** When content naturally belongs to distinct categories that visitors would want to browse independently. For a blog, workspaces might be "Engineering", "Design", "Product". For a recipe site, "Breakfast", "Dinner", "Desserts".

### Validation Actions

Types can have validation actions (e.g., "Peer Reviewed", "Editor Approved") that mark content as human-validated. This supports E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) signals for SEO. Use `validate_item` to apply a validation action to an item.

## Settings Optimization

Use `update_settings` to configure site behavior. Settings are key-value pairs. The `get_metadata` response tells you current settings. Key categories:

- **Identity:** `source_title`, `source_description`, `owner_name`, `owner_github`
- **Theme:** `source_color_primary`, `source_color_secondary`, `source_color_background`, `source_color_text`, `source_color_muted`
- **Quality:** `quality_guidelines` (JSON), `validation_limits` (JSON)
- **SEO:** CTA settings (`cta_hero_*`, `cta_post_*`, `cta_sticky_*`)
- **Integration:** `webhook_url`, `webhook_events`, `webhook_secret`
- **Custom CSS:** `source_custom_css`

Settings changes take effect immediately -- no redeployment needed. This is a key distinction: settings are stored in D1 and read at runtime, while code changes require `redeploy_site`.

## When to Redeploy vs Reconfigure

**Redeploy** (`redeploy_site`) when:
- A new platform version has been released with bug fixes or features
- The template code has been updated (custom repo only)
- You want to pick up the latest CSS/JS assets

**Reconfigure** (via `update_settings`, `create_type`, `create_workspace`, etc.) when:
- Changing site title, description, or theme colors
- Adding or modifying content types or workspaces
- Updating quality guidelines or validation limits
- Configuring webhooks or CTAs

Redeployment is a heavier operation (30-60 seconds, involves downloading and uploading the worker bundle). Settings changes are instant. Always prefer reconfiguration over redeployment when possible.

## Site Actions (Forms)

Site actions are dynamic forms (contact forms, newsletter signup, lead capture). They are managed via `create_action`, `update_action`, `list_actions`, `delete_action`. Submissions are managed via `list_submissions`, `manage_submission`, `delete_submission`, `get_submission_stats`, `export_submissions`.

Forms can be embedded in content using the `{{action:slug}}` directive in any item's markdown content. They can also be accessed directly at `/form/:slug` on the site.

## Further Reading

- [MCP Workflow Reference](references/mcp-workflow.md) -- Complete tool reference and common multi-step workflows
- [Troubleshooting Guide](references/troubleshooting.md) -- Common issues and resolution patterns
