# Content Quality Review and Publishing Workflow

Content quality review and publishing workflow for any Pignal site. Review methodology, quality gates, content lifecycle management, and bulk moderation. Use when reviewing content, checking quality, moderating posts, auditing content, or deciding what to publish. Also use for content cleanup, archiving, or bulk operations.

---

## The MCP Workflow

Every content review operation follows this sequence. Do not skip steps.

1. **Discover** -- `get_site_tools` to learn what the site can do.
2. **Learn the site** -- `call_site_tool` with `get_metadata` to get types, workspaces, quality guidelines, and validation limits. This is not optional. Quality criteria vary per site.
3. **Inventory** -- `call_site_tool` with `list_items` to see existing content. Filter by type, workspace, or archived status to focus the review.
4. **Assess** -- Apply the quality heuristics below to each item. Score clarity, structure, and value.
5. **Act** -- Validate, publish, update, or archive based on the assessment.
6. **Report** -- Summarize findings: what was published, what needs work, what was archived.

---

## Quality Heuristics

Every piece of content should be evaluated against three dimensions. These are not binary pass/fail -- they exist on a spectrum. The goal is to push content toward the right end before publishing.

### Clarity

Does the reader understand the point within the first two sentences?

**Strong signals:**
- The keySummary captures the core insight without reading the content
- The opening paragraph names a specific problem, discovery, or decision
- Technical terms are used precisely (not vaguely to sound authoritative)
- Sentences are direct -- subject-verb-object, minimal passive voice

**Weak signals:**
- The keySummary is vague ("Thoughts on X") or repeats the first sentence
- The opening paragraph is throat-clearing ("In this post, I will discuss...")
- Jargon is used without context or definition
- Sentences require re-reading to parse

### Structure

Can a reader scan the content and extract 80% of the value?

**Strong signals:**
- Headings function as complete thoughts, not labels
- Parallel items use bullet lists; sequential steps use numbered lists
- Code blocks have language tags and relevant comments
- Paragraphs are short (2-4 sentences) with one idea each
- Tables are used for structured comparisons

**Weak signals:**
- Headings are single words ("Background", "Details")
- Long prose blocks with no structural breaks
- Code without context or language tags
- Paragraphs that mix multiple unrelated points

### Value

Would the target audience bookmark this?

**Strong signals:**
- Contains specific data, examples, or experience (not just opinions)
- Addresses a real problem the audience faces
- Provides actionable takeaways
- Acknowledges tradeoffs honestly

**Weak signals:**
- Restates commonly known information
- Contains only abstract advice with no specifics
- Makes claims without supporting evidence or experience
- Reads like a summary of documentation rather than original thought

---

## Review Methodology

### Single Item Review

When reviewing one piece of content, follow this sequence:

1. **Read the keySummary alone.** Does it communicate the core value? Would you click it in a feed?
2. **Scan the headings.** Do they tell a coherent story without reading the body?
3. **Read the opening.** Does it hook within two sentences?
4. **Check the body.** Does every section earn its place? Could anything be cut without losing information?
5. **Read the closing.** Does it crystallize a takeaway? Is the takeaway actionable?
6. **Verify metadata.** Is the content type correct? Are tags relevant and reusable? Is the workspace appropriate?

### Batch Review

When auditing multiple items, prioritize efficiency:

1. Call `list_items` with a generous limit to see everything. Note total counts per type.
2. Sort mentally into three buckets: **ready to publish**, **needs minor edits**, **needs significant work or archiving**.
3. For ready items, use `batch_vouch_items` to publish in one call.
4. For items needing edits, use `update_item` to fix keySummary, content, type, or tags.
5. For items that should be retired, use `archive_item` -- prefer archiving over deleting.

### Review Rubric

Score each item 1-5 on each dimension:

| Score | Clarity | Structure | Value |
|-------|---------|-----------|-------|
| 1 | Incomprehensible | Wall of text | No useful information |
| 2 | Confusing, requires effort | Some structure but disorganized | Restates obvious knowledge |
| 3 | Understandable but unfocused | Adequate headings and breaks | Useful but not distinctive |
| 4 | Clear with minor rough spots | Well-organized and scannable | Contains specific insights |
| 5 | Crystal clear from first sentence | Exemplary structure and flow | Would bookmark and share |

**Publishing threshold:** A combined score of 10+ (average 3.3 per dimension) with no dimension below 3 is the minimum for vouching. Items scoring below this need editing or should remain private.

---

## Visibility Strategy

Pignal uses three visibility tiers. The progression matters because vouched items get permanent URL slugs that search engines index. Changing slugs after publication breaks links and SEO equity.

- **`private`** (default) -- Content in draft or review. Only accessible via API/MCP.
- **`unlisted`** -- Accessible via share token URL (`/s/:token`). Use for pre-publication review or content shared with a specific audience.
- **`vouched`** -- Public, listed on the site, SEO-indexed with a permanent slug. Reserve for finalized content.

**Decision framework:**
- Is the content complete and reviewed? Vouch it.
- Is the content complete but you want specific feedback first? Make it unlisted.
- Is the content still in progress? Keep it private.
- Is the content outdated but might be useful later? Archive it.
- Is the content wrong, irrelevant, or harmful? Delete it.

---

## Validation Workflow (E-E-A-T)

Validation actions mark content as human-reviewed. This is not a rubber stamp -- it is a meaningful quality signal that appears on the public page and in JSON-LD structured data. Each validation action represents a specific kind of assessment.

### The Validation Sequence

1. **Check available actions** -- `get_metadata` lists validation actions per type. Each action has a label (e.g., "Confirmed", "Peer Reviewed", "Editor Approved").
2. **Evaluate honestly** -- Choose the action that reflects the actual assessment performed. Do not apply "Confirmed" if you have not verified the claims.
3. **Apply** -- `call_site_tool` with `validate_item`, providing the `itemId` and `actionId`.

### E-E-A-T Alignment

Pignal's validation system directly supports Google's E-E-A-T quality signals:

- **Experience** -- First-person framing in content signals lived experience. Validation confirms this is genuine.
- **Expertise** -- Type-specific validation actions (e.g., "Technically Verified") signal domain knowledge.
- **Authoritativeness** -- The author attribution chain (owner_name, source_title, domain) establishes identity.
- **Trustworthiness** -- Public validation labels and the `creativeWorkStatus` field in JSON-LD signal editorial rigor.

### When to Validate

- Before vouching any item to `vouched` visibility
- After significant content updates
- During periodic content audits to refresh validation timestamps

---

## Content Lifecycle

Content moves through a predictable lifecycle. Understanding each phase prevents common mistakes.

### Draft Phase

Content is created via `save_item` and starts as `private`. During this phase:
- Focus on completeness, not polish
- Assign the correct type and workspace
- Add relevant tags early -- they are easier to add during creation than retroactively
- Do not worry about the slug yet -- it is set when vouching

### Review Phase

Content is ready for assessment. The review can be self-review or collaborative:
- Apply the quality heuristics (clarity, structure, value)
- Check that the keySummary fits within the site's configured limits (from `get_metadata`)
- Verify that content length respects the configured maximum
- Ensure type selection matches the content's shape (insight, decision, solution, reference)

### Publish Phase

Content passes review and is ready for the public:
- Apply a validation action via `validate_item` to mark it as reviewed
- Vouch with `vouch_item`, providing `visibility: "vouched"` and optionally a clean slug
- Slug best practices: lowercase, hyphenated, descriptive, no dates. Example: `react-server-components-data-fetching`

### Update Phase

Published content may need updates over time:
- Use `update_item` to modify keySummary, content, type, workspace, or tags
- After significant updates, re-validate to refresh the validation timestamp
- Do not change slugs on published items unless absolutely necessary -- it breaks existing links

### Archive Phase

Content that is no longer current but has historical value:
- Use `archive_item` to soft-remove from listings while preserving the data
- Archived items can be restored with `unarchive_item` if needed
- Prefer archiving over deleting. Deletion is permanent.

---

## Stale Content Detection

Content decays. Technical posts reference outdated versions. Tutorials describe deprecated APIs. Regular audits prevent a site from becoming a graveyard of stale information.

### Staleness Indicators

- **Version references** -- Content mentions specific software versions. If the current version has moved significantly, the content may be stale.
- **Time markers** -- Phrases like "recently", "this year", "just released" decay rapidly.
- **Broken patterns** -- The technique described no longer works or has been superseded by a better approach.
- **Low engagement** -- Content that once attracted visitors but no longer does may have been outpaced by newer resources.

### Audit Cadence

- **Quarterly:** Review all vouched content for staleness signals. Update version references, refresh examples, or archive items that cannot be salvaged.
- **On redeployment:** After updating a site's template, review content for any new formatting issues or broken directives.
- **After major releases:** When a technology or tool referenced in content ships a major version, review all content tagged with that technology.

### Audit via MCP

```
1. call_site_tool(id, "list_items", { limit: 100 })
2. For each item, check:
   - Is the type still appropriate?
   - Are the tags still accurate?
   - Does the keySummary still reflect the content?
   - Is the content still technically accurate?
3. For stale items: update_item with corrections or archive_item
4. For items needing validation refresh: validate_item again
```

---

## Bulk Moderation

When managing content at scale, efficiency matters. Use these patterns for common bulk operations.

### Bulk Publish

For multiple items ready to go public:

```
1. call_site_tool(id, "list_items", { limit: 50 })  -- find private items
2. Review each for publishing readiness
3. call_site_tool(id, "batch_vouch_items", {
     items: [
       { itemId: "...", visibility: "vouched", slug: "descriptive-slug" },
       { itemId: "...", visibility: "vouched" }
     ]
   })
```

`batch_vouch_items` returns a summary of successes and failures. Check the response for any items that failed (usually slug conflicts) and handle individually.

### Bulk Archive

For retiring outdated content:

```
1. call_site_tool(id, "list_items", { limit: 100 })
2. Identify items for archiving (stale, superseded, or low-value)
3. For each: call_site_tool(id, "archive_item", { itemId: "..." })
```

There is no batch archive tool -- archive items individually. This is intentional: archiving should be a deliberate decision per item.

### Bulk Tag Cleanup

When tags have become inconsistent:

```
1. call_site_tool(id, "list_items", { limit: 100 })  -- review existing tags
2. Identify tag inconsistencies (e.g., "js" vs "javascript", "react-hooks" vs "hooks")
3. For each item needing tag updates:
   call_site_tool(id, "update_item", { itemId: "...", tags: ["corrected", "tags"] })
```

---

## References

For deeper exploration of specific topics:

- **[Review Methodology](references/review-methodology.md)** -- Systematic review processes, quality rubrics, and editorial standards for different content types and site scales.
- **[Quality Checklist](references/quality-checklist.md)** -- Universal quality checks to apply before publishing any content, regardless of type or template.
