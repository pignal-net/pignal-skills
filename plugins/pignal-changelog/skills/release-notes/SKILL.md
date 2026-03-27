---
name: release-notes
description: |
  Release communication craft for Pignal Changelog sites. Keep a Changelog
  conventions, semver principles, change categorization, and audience-appropriate
  release notes. Use this skill when writing changelogs, documenting releases,
  creating release notes, summarizing what shipped, or operating a Pignal
  changelog site. Also triggers on version updates, release documentation,
  or "what changed" requests.
---

# Release Communication Craft

A changelog is a contract with your users: "Here's exactly what changed, categorized so you can find what matters to you." The craft is in communicating changes clearly to the right audience — developers need different information than end users.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (change categories), workspaces (product areas), and limits

## Keep a Changelog Conventions

The [Keep a Changelog](https://keepachangelog.com/) standard exists because ad-hoc changelogs become unreadable. Its conventions solve real problems:

### Change Categories

- **Added** — new features users didn't have before
- **Changed** — modifications to existing functionality
- **Deprecated** — features that will be removed (early warning)
- **Removed** — features that are gone
- **Fixed** — bug corrections
- **Security** — vulnerability patches (always call these out explicitly because users may need to act urgently)

**Why categorization matters:** A user upgrading from v2.3 to v2.5 needs to quickly scan for breaking changes and security fixes. Without categories, they'd have to read every line.

### Version Ordering

Newest version first. Users looking at a changelog typically want to know "what changed recently," not "what happened at the beginning."

### Semver Principles

- **Major (X.0.0):** Breaking changes — something that worked before won't work the same way
- **Minor (0.X.0):** New functionality that's backward compatible
- **Patch (0.0.X):** Bug fixes and minor improvements

**Why semver matters:** It's a communication protocol. When a user sees a major version bump, they know to check for breaking changes. A patch bump signals "safe to update without reading notes."

## Audience-Appropriate Language

### Developer Changelogs

Technical, specific, linked to implementation:
- "Fixed race condition in WebSocket reconnection when multiple tabs are open (#1234)"
- "Added `--dry-run` flag to migration command"
- Link to PRs, issues, and commits

### User-Facing Release Notes

Benefit-oriented, jargon-free:
- "Fixed an issue where notifications could appear twice"
- "You can now export your data as CSV"
- Focus on what the user can do differently, not how you implemented it

**The same change, two audiences:**
- Developer: "Refactored session store from in-memory to Redis (#892)"
- User: "Improved reliability of login sessions — you'll be logged out less frequently"

## Breaking Change Communication

Breaking changes need special treatment because they require user action.

### The Pattern

1. **What changed** — specific, not vague
2. **Why it changed** — the reasoning (users accept changes better when they understand why)
3. **What to do** — concrete migration steps
4. **Timeline** — when the old way stops working (if applicable)

### Deprecation Before Removal

Whenever possible, deprecate before removing:
1. Version N: Mark as deprecated, explain the replacement
2. Version N+1 or N+2: Remove the deprecated feature

This gives users time to migrate without surprise breakage.

## Writing Effective Entries

### Be Specific

- **Vague:** "Improved performance"
- **Specific:** "Reduced API response time for list endpoints by 40% through query optimization"

### Lead with the User Impact

- **Implementation-first:** "Migrated from Webpack to Vite"
- **Impact-first:** "Development server starts 10x faster (migrated build tool from Webpack to Vite)"

### Group Related Changes

If a release has many changes, group by area or component:

```markdown
### Added
#### API
- New `/export` endpoint for bulk data export
- Rate limit headers now included in all responses

#### Dashboard
- Dark mode support
- Keyboard shortcuts for common actions
```

## Common Mistakes

- **"Various bug fixes and improvements"** — This tells users nothing. If changes aren't worth listing, they might not be worth releasing.
- **Missing dates** — Always include the release date. Users need to know when a change shipped.
- **Inconsistent formatting** — Pick a format and stick to it. Inconsistency makes changelogs harder to scan.
- **Forgetting security disclosures** — Security fixes should always be called out clearly so users can assess urgency.

## Workflow

1. **Discover** — `get_metadata` for types and organization
2. **Draft** — `save_item` with version as keySummary, categorized changes in content
3. **Categorize** — Use the right change type (Added/Changed/Fixed/etc.)
4. **Review** — Check audience appropriateness, specificity, breaking change docs
5. **Publish** — `validate_item` then `vouch_item`

For deeper understanding of the standard, see [keep-a-changelog.md](references/keep-a-changelog.md).
