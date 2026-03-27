# Review Methodology

Systematic review processes, quality rubrics, and editorial standards for content review at any scale.

## The Three Review Modes

Different situations call for different levels of review rigor. Match the mode to the context.

### Quick Scan (2-3 minutes per item)

Use for: bulk imports, draft triage, familiar content types.

1. Read the keySummary -- does it promise clear value?
2. Scan headings -- do they tell a coherent story?
3. Check length -- is the content appropriately sized for its type?
4. Verify metadata -- correct type, reasonable tags, appropriate workspace.
5. Decision: publish, flag for deeper review, or archive.

This mode prioritizes throughput. It catches obvious problems (wrong type, missing keySummary value, broken structure) but does not deeply evaluate accuracy or completeness.

### Standard Review (5-10 minutes per item)

Use for: regular publishing workflow, items heading to `vouched` visibility.

1. **keySummary assessment** -- Is it specific? Does it fit the type's pattern? Within character limits?
2. **Opening evaluation** -- Does the first paragraph hook the reader with a specific problem or insight?
3. **Structure check** -- Are headings descriptive? Lists used appropriately? Code blocks properly tagged?
4. **Content quality** -- Does every paragraph contribute? Are claims supported? Are tradeoffs acknowledged?
5. **Closing review** -- Is there an actionable takeaway?
6. **Metadata verification** -- Type, workspace, tags all correct and consistent with site conventions.
7. **Validation** -- Apply the appropriate validation action.
8. **Slug review** -- For vouching, ensure the slug is clean, descriptive, and permanent-worthy.

### Deep Audit (15-30 minutes per item)

Use for: flagship content, high-traffic items, content audits, items flagged during standard review.

Everything from standard review, plus:

1. **Technical accuracy** -- Are code examples runnable? Are version references current? Are API calls correct?
2. **Completeness** -- Are there obvious gaps? Would a reader need to search elsewhere to complete the task?
3. **Competitive analysis** -- Does this add value beyond what already exists on the topic? What is the unique angle?
4. **SEO assessment** -- Does the keySummary target a searchable query? Are headings written as answers? Would a heading work as a featured snippet?
5. **Cross-reference** -- Does this content connect to other items on the site? Should internal references be added?
6. **Freshness** -- When was this written? Are time-sensitive references still accurate?

## Quality Rubrics by Content Shape

Different content shapes have different quality standards. Use the shape that matches the content, not the template.

### Insight / Discovery

The author learned something new and is sharing the learning.

| Criterion | Good | Needs Work |
|-----------|------|------------|
| keySummary | States the specific insight | Vague topic reference |
| Opening | Names the moment of discovery | Generic introduction |
| Body | Explains why this matters and what changed | Just states facts without context |
| Evidence | Personal experience, specific data | Unsupported claims |
| Takeaway | What the reader should understand differently | Summary of what was said |

### Decision / Choice

The author chose between alternatives and explains the reasoning.

| Criterion | Good | Needs Work |
|-----------|------|------------|
| keySummary | Names the choice and the reason | Just names the topic |
| Alternatives | Lists what was considered and why each was rejected | Only describes the winner |
| Criteria | Explicitly states what mattered (performance, DX, cost) | Implicit or missing criteria |
| Tradeoffs | Honestly names what was given up | Presents the choice as universally correct |
| Context | Clarifies when this decision applies (and when it doesn't) | Implies it applies everywhere |

### Solution / Fix

The author solved a specific problem and documents the fix.

| Criterion | Good | Needs Work |
|-----------|------|------------|
| keySummary | Problem -> Solution in one line | Just the problem or just the solution |
| Symptoms | Describes what the reader would observe | Jumps straight to the fix |
| Root cause | Explains why the problem occurs | Treats the fix as magic |
| Fix steps | Numbered, sequential, verifiable | Vague instructions |
| Verification | How to confirm the fix worked | Ends at "apply the fix" |

### Reference / Documentation

The author documents facts, APIs, or patterns for lookup.

| Criterion | Good | Needs Work |
|-----------|------|------------|
| keySummary | Names the subject and scope | Vague title |
| Organization | Tables, lists, code blocks for density | Paragraphs of prose |
| Completeness | Covers the stated scope fully | Obvious gaps |
| Examples | Runnable code for key concepts | No examples or broken code |
| Currency | Version-specific, dated where relevant | Undated, may be stale |

## Editorial Standards

### Voice Consistency

A site should have a consistent voice. During review, check that:
- The author perspective is consistent (first person vs. third person)
- The formality level matches other published content
- Technical depth is appropriate for the site's audience
- Tone is appropriate (conversational, professional, academic)

### Formatting Consistency

- Heading levels follow the same hierarchy across items (H2 for sections, H3 for subsections)
- Code blocks consistently use language tags
- Lists use the same conventions (bullet for unordered, numbered for sequential)
- Tags follow the site's existing taxonomy (check with `list_items` to see existing tags)

### Metadata Consistency

- Types are used according to their `whenToUse` guidance (from `get_metadata`)
- Workspaces group content logically and consistently
- Tags are lowercase, single-concept, and reuse existing tags where possible
- keySummary length stays within the site's configured limits

## Scaling Review

### For Small Sites (under 50 items)

Review every item individually using standard review. At this scale, quality matters more than throughput because each item represents a larger fraction of the site's total value.

### For Medium Sites (50-200 items)

Use quick scan for the full inventory, then standard review for items flagged during scanning. Focus deep audits on the top 10% of content by traffic or strategic importance.

### For Large Sites (200+ items)

- Quick scan the most recent 50 items (newest content is most likely to need attention)
- Standard review items that are about to be published
- Deep audit on a rotating basis: review 10-20 items per audit cycle, prioritizing high-traffic and aging content
- Use `search_items` to find items by tag or keyword when investigating specific quality concerns
