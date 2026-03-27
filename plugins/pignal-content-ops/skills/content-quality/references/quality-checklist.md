# Quality Checklist

Universal quality checks to apply before publishing any content. Use this as a final gate before vouching items to public visibility.

## Pre-Publish Checklist

Run through every item on this list before calling `vouch_item`. A single "no" does not necessarily block publication, but multiple "no" answers should trigger a return to editing.

### keySummary

- [ ] Is the keySummary specific enough that a reader knows what they will learn before clicking?
- [ ] Does it fit within the site's configured character limits (check `get_metadata`)?
- [ ] Does it match the content type's recommended pattern (e.g., "How to..." for tutorials, "Problem -> Solution" for fixes)?
- [ ] Does it avoid clickbait, vagueness, and unnecessary cleverness?
- [ ] Does it front-load the most important words (search results truncate at ~60 characters)?

### Opening

- [ ] Does the first paragraph name a specific problem, insight, or question?
- [ ] Can the reader answer "why should I keep reading?" after the first two sentences?
- [ ] Does it avoid throat-clearing openers ("In this post...", "Today I want to discuss...")?

### Structure

- [ ] Do headings function as complete thoughts, not single-word labels?
- [ ] Does the heading hierarchy follow H2 -> H3 without skipping levels?
- [ ] Are parallel items in bullet lists and sequential steps in numbered lists?
- [ ] Are paragraphs short (2-4 sentences) with one idea each?
- [ ] Are code blocks tagged with the correct language?
- [ ] Are tables used for structured comparisons where appropriate?

### Content Quality

- [ ] Does every section contribute information the reader needs? Could anything be cut?
- [ ] Are claims supported with specific evidence, data, or experience?
- [ ] Are tradeoffs acknowledged honestly?
- [ ] Is the technical content accurate and current?
- [ ] Are version numbers and time references explicit (not "recently" or "the new version")?

### Closing

- [ ] Does the final section crystallize a specific takeaway?
- [ ] Is the takeaway actionable -- does it change what the reader does next?
- [ ] Does it avoid weak endings ("In conclusion...", "I hope this was helpful")?

### Metadata

- [ ] Is the content type correct for the shape of the content (insight, decision, solution, reference)?
- [ ] Is the workspace appropriate for the content's topic?
- [ ] Are tags lowercase, single-concept, and consistent with existing site tags?
- [ ] Are there 2-5 relevant tags (not too few, not too many)?
- [ ] Has a validation action been applied via `validate_item`?

### Slug (for vouching)

- [ ] Is the slug lowercase and hyphenated?
- [ ] Is it descriptive enough to convey the topic from the URL alone?
- [ ] Does it avoid dates (URLs should be permanent; dates make them look stale)?
- [ ] Is it concise (3-6 words)?
- [ ] Is it unique (no conflict with existing slugs)?

## Content Type-Specific Checks

### For Insights / Discoveries

- [ ] Is the "aha moment" clear in the keySummary?
- [ ] Does the content explain what changed in the author's understanding?
- [ ] Is the insight grounded in specific experience, not abstract theory?

### For Decisions / Choices

- [ ] Are the alternatives named and compared?
- [ ] Are the selection criteria explicit?
- [ ] Is the context clarified (when does this decision apply)?

### For Solutions / Fixes

- [ ] Are symptoms described (what would the reader observe)?
- [ ] Is the root cause explained (not just the fix)?
- [ ] Are fix steps sequential and verifiable?
- [ ] Is there a verification step (how to confirm the fix worked)?

### For References / Documentation

- [ ] Is the scope clearly stated?
- [ ] Are tables and lists used for density?
- [ ] Are code examples runnable?
- [ ] Is the content version-specific where relevant?

## Common Issues to Catch

### keySummary Problems

| Problem | Example | Fix |
|---------|---------|-----|
| Too vague | "Database thoughts" | "Why I chose SQLite over Postgres for edge deployment" |
| Too long | Exceeds site's max limit | Trim to the essential insight, move detail to content |
| Repeats content | keySummary is verbatim first line | Rewrite to be complementary, not identical |
| Wrong tone | Clickbait or sensational | Rewrite to be specific and honest |

### Structure Problems

| Problem | Symptom | Fix |
|---------|---------|-----|
| Wall of text | No headings, long paragraphs | Add H2/H3 sections, break paragraphs |
| Heading soup | Too many headings, no flow | Merge related sections, reduce nesting |
| Wrong list type | Numbered list for unordered items | Use bullets for unordered, numbers for sequential |
| Missing code tags | Code blocks without language | Add language tag for syntax highlighting |

### Metadata Problems

| Problem | Symptom | Fix |
|---------|---------|-----|
| Wrong type | Decision content tagged as Insight | Change typeId to match the content's shape |
| Tag sprawl | 8+ tags, many used once | Reduce to 2-5, reuse existing tags |
| Missing workspace | Content not organized | Assign to appropriate workspace |
| No validation | Publishing without review | Apply validate_item first |

## Post-Publish Verification

After vouching, verify:

1. Call `list_items` to confirm the item appears with `vouched` visibility
2. Check that the slug was set correctly
3. If the site has CTA settings enabled, the item will automatically display CTAs -- no extra action needed
4. If the content contains directives (`{{action:slug}}`, `{{cta:...}}`), verify the referenced forms exist and are active
