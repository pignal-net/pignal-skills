# Blogging Craft

Expert blog writing, editorial workflow, and content strategy for blog and signals sites. Use this skill whenever creating blog posts, planning content, structuring articles, choosing content types, designing tag taxonomies, or managing an editorial pipeline. Covers the full lifecycle: ideation, drafting, structuring, tagging, validating, and publishing.

---

## The MCP Workflow

Every blog operation follows this sequence. Do not skip steps — each one prevents a different class of mistake.

1. **Discover** — `get_site_tools` to learn what the site can do.
2. **Learn the site** — `call_site_tool` with `get_metadata` to get the site's types, workspaces, limits, and quality guidelines. This is not optional. Types, workspaces, and field limits vary per site and are configured by the site owner. Never assume them.
3. **Plan** — Decide content type, workspace, tags, and structure based on what `get_metadata` returned. Match the *shape of thinking* to a type (see "Choosing the Right Content Type" below).
4. **Draft** — Write the keySummary and markdown content following the craft principles in this skill.
5. **Create** — `call_site_tool` with `save_item` to save the post.
6. **Validate** — `call_site_tool` with `validate_item` to apply a quality action (e.g., Confirmed, Worked). This forces an accuracy evaluation and strengthens the post's credibility.
7. **Publish** — `call_site_tool` with `vouch_item` to set visibility to `vouched` with a clean URL slug.

---

## Blog Writing Craft

### The Hook-Substance-Takeaway Structure

Every effective blog post follows this three-part arc. It is not a rigid template — it is a rhythm that holds reader attention from first sentence to last.

**Hook (first 2-3 sentences):** State the problem, insight, or question that makes the reader care. The hook answers "why should I keep reading?" If the reader cannot answer that question after the first paragraph, the post has already failed.

Why this matters: Readers decide within 5 seconds whether to stay or leave. A hook that names a specific pain point ("I spent 3 hours debugging a memory leak caused by a missing cleanup function") outperforms a vague opening ("Today I want to talk about React hooks") because it signals *density of value*.

**Substance (the body):** Deliver the promised value. This is where structure matters most — see "Scannable Structure" below. Every paragraph should advance the reader's understanding. If a paragraph could be removed without losing information, remove it.

**Takeaway (final paragraph):** Crystallize the single most important thing the reader should remember. This is not a summary — it is the one sentence you would want quoted if someone shared your post. Good takeaways are actionable: they change what the reader does next.

### First-Person Framing

Write from personal experience using "I" and "My" framing. This is not a stylistic preference — it serves three practical purposes:

1. **Authenticity signal.** "I discovered that PKCE is required for mobile OAuth" is verifiable against the author's experience. "PKCE is required for mobile OAuth" is an unsourced claim that competes with every other unsourced claim on the internet.
2. **E-E-A-T alignment.** Search engines explicitly reward Experience and Expertise signals. First-person framing naturally encodes both.
3. **Recall value.** When the author revisits their own posts months later, first-person framing triggers episodic memory ("I remember working through this") rather than semantic memory ("this is a fact I read somewhere"). This makes personal knowledge bases dramatically more useful over time.

When to break this rule: Reference articles and pure technical specs should use neutral voice. The content type determines voice — see "Choosing the Right Content Type."

### Scannable Structure

Most readers scan before they read. Structure your post so that scanning delivers 80% of the value:

**Headings as signposts.** Each heading should be a complete thought, not a label. "Why connection pooling matters for serverless" beats "Connection Pooling" because a scanner gets value without reading the section.

**Lists for parallel items.** Use bullet lists for unordered groups (options, features, alternatives). Use numbered lists for sequential steps. Never use a numbered list when order does not matter — it implies false priority.

**Tables for structured comparisons.** When comparing options, technologies, or approaches, a table communicates density that paragraphs cannot. A 5-row comparison table replaces 10 paragraphs of prose.

**Code blocks for anything executable.** Commands, config snippets, error messages, API responses — anything the reader might copy-paste belongs in a fenced code block with a language tag. Always include expected output after commands when the output is meaningful.

**Short paragraphs.** Web paragraphs should rarely exceed 4 sentences. Each paragraph should make one point. If you find yourself writing "Additionally" or "Furthermore," you are probably combining two points that deserve separate paragraphs.

Why this matters: Blog analytics consistently show that structured posts have 2-3x longer read times than wall-of-text posts with the same word count. Scannable structure respects the reader's time and earns their attention.

---

## Choosing the Right Content Type

The site's `get_metadata` response lists available content types with guidance for each. The key decision is matching the *shape of your thinking* to the right type — not the topic.

The same topic can be multiple types. "React Server Components" could be an insight ("I learned RSCs eliminate client-server waterfalls"), a decision ("Chose RSCs over client components for our data-heavy dashboard"), a solution ("Fixed hydration mismatch by moving useState to a client component boundary"), or a reference ("React Server Components: API surface and constraints").

**Shape-based selection principles:**

- **Did you learn something new?** The insight/discovery shape. The keySummary should capture the "aha" moment. Content explains what changed in your understanding and why it matters.
- **Did you choose between alternatives?** The decision shape. The keySummary names the choice and the reason. Content compares options, states criteria, and explains the winner.
- **Did you solve a specific problem?** The solution/fix shape. The keySummary is Problem -> Solution in one line. Content walks through symptoms, root cause, fix steps, and verification.
- **Are you documenting facts for lookup?** The reference shape. The keySummary names the subject and scope. Content uses tables, lists, and code blocks for density — minimal narrative.

Always read the `guidance` field from `get_metadata` for each type. Site owners customize patterns and hints to match their domain. Follow them.

---

## Tagging Strategy

Tags serve two purposes: they help readers discover related posts, and they create filtered views that function as mini-collections. A good tagging strategy makes both work.

### Tag Design Principles

**Use lowercase, single-concept tags.** Tags like `react`, `performance`, `oauth` work. Tags like `React Performance Tips` do not — they are titles pretending to be tags.

**Aim for 2-5 tags per post.** Fewer than 2 means the post is under-categorized and will not appear in filtered views. More than 5 means the tags are too granular to be useful filters.

**Think in reader questions.** A reader browsing by tag is asking "show me everything about X." Your tags should answer questions like: What technology? (`react`, `cloudflare`). What concern? (`performance`, `security`). What activity? (`debugging`, `architecture`).

**Reuse existing tags aggressively.** Before creating a new tag, check `list_items` or `search_items` to see which tags already exist on the site. A tag with 1 post is barely a tag. A tag with 10 posts is a collection. Prefer existing tags over new ones.

**Avoid meta-tags.** Tags like `blog`, `post`, `article` add no filtering value — every item on the site is already a blog post.

---

## Series and Sequencing

Some topics are too large for one post but too connected to scatter across unrelated entries.

### When to Split

Split when the post exceeds 2,000 words AND covers distinct sub-topics that a reader might want independently. A 3,000-word post about "Setting up a full CI/CD pipeline" naturally splits into "Configuring GitHub Actions" and "Deploying to Cloudflare Workers" because each sub-topic has standalone value.

### When to Combine

Combine when splitting would force the reader to read multiple posts to understand one thing. A 2,500-word post about "Why we chose SQLite over Postgres for edge deployment" should stay as one piece even if it covers benchmarks, developer experience, and operational concerns — because the argument requires all three.

### Sequencing Strategy

For a series, use consistent tag grouping and include navigation hints in the content:
- Reference previous posts by title (the platform does not support internal linking natively, but mentioning titles helps readers search).
- Number series posts in the keySummary when order matters: "Part 1: ...", "Part 2: ..."
- Use the same workspace for all posts in a series so they appear together in filtered views.

---

## Evergreen vs. Topical Content

**Evergreen posts** remain useful for months or years. They address principles, patterns, and foundational concepts. Examples: "How I structure React component hierarchies" or "My decision framework for choosing databases." Invest more time in these — they compound in value.

**Topical posts** address current events, new releases, or time-sensitive discoveries. Examples: "First impressions of React 19" or "Workaround for the Cloudflare D1 batch limit." These are valuable when fresh but decay. Mark versions explicitly ("As of React 19.0.0-rc.1") so future readers know the context.

**The balance:** A healthy blog is roughly 70% evergreen, 30% topical. Topical posts bring traffic now; evergreen posts bring traffic forever.

---

## Reader Engagement Principles

Write as if you are explaining to a respected colleague — not lecturing a student and not dumbing down for a beginner.

**Be specific over general.** "Use connection pooling" is generic advice. "In our Cloudflare Workers setup, adding PgBouncer reduced cold-start latency from 800ms to 120ms" is a signal worth reading.

**Show your reasoning, not just your conclusion.** Readers trust authors who show how they arrived at a decision. "We chose Hono over Express because..." is more valuable than "Use Hono."

**Acknowledge tradeoffs.** Nothing is universally best. Naming what you gave up ("We accepted a larger bundle size in exchange for type safety") builds credibility and helps readers evaluate whether your solution fits their context.

**Include failure.** Posts about what went wrong are often more useful than posts about what went right. "Why our caching strategy backfired" teaches readers to avoid a mistake they might otherwise make.

---

## Markdown Best Practices for Web

### Heading Hierarchy

Use H2 (`##`) for major sections and H3 (`###`) for subsections. Never skip levels (no H2 directly to H4). The heading hierarchy creates the table of contents that blog templates render as sidebar navigation.

### Code Blocks

Always specify the language tag for syntax highlighting:

````
```typescript
const result = await fetch('/api/items');
```
````

For shell commands, use `bash` or `sh` and include the expected output when it helps understanding:

````
```bash
curl -s https://example.com/.well-known/pignal | jq .owner
# { "github_login": "username", "display_name": "Name" }
```
````

### Links and References

When referencing external resources, use descriptive link text that tells the reader what they will find. "[Cloudflare's connection pooling docs](url)" beats "[click here](url)."

### Images

If the post references visual output (charts, screenshots, diagrams), describe what the reader would see. The platform supports markdown images, but only if the image is hosted externally.

---

## The keySummary

The keySummary is the post's title — the first thing readers see in the feed and the primary text search engines index. Invest time here.

**Good keySummary patterns:**
- First-person discovery: "I learned that D1 batch operations have a 500-row limit"
- Decision with reason: "Chose edge-side rendering over SSR for our dashboard"
- Problem-solution: "Fixed CORS errors by moving headers to the Worker, not the origin"
- Reference scope: "React Hook Rules: The complete constraint set"

**keySummary anti-patterns:**
- Vague: "Thoughts on databases" (no signal about what thoughts or which databases)
- Clickbait: "You won't believe what happened with our deploy" (breaks trust)
- Too long: Keep within the site's character limits (check `get_metadata`)
- Repeating content: The keySummary should not appear verbatim as the first line of the content body

---

## Publishing Workflow

After creating a post with `save_item`, it starts as `private`. Move through the quality gates:

1. **Validate** — Use `validate_item` to apply a quality action. This is a meaningful assessment, not a rubber stamp. Choose the action that honestly reflects the post's status.
2. **Review the slug** — When publishing with `vouch_item`, provide a clean URL slug. Good slugs are lowercase, hyphenated, and descriptive: `react-server-components-data-fetching`. Avoid dates in slugs (URLs should be permanent; dates make them look stale).
3. **Publish** — Set visibility to `vouched` to make the post public, listed in the feed, and indexed by search engines.

For batch publishing, use `batch_vouch_items` to publish multiple posts at once — useful when launching a series.

---

## References

For deeper exploration of specific topics:

- **[Editorial Craft](references/editorial-craft.md)** — Content planning, editorial calendar principles, voice consistency, and the draft-review-publish cycle.
- **[Content Strategy](references/content-strategy.md)** — Audience-first planning, content pillars, frequency, repurposing, and measuring effectiveness.
- **[Article SEO](references/article-seo.md)** — Title optimization, heading hierarchy for crawlers, internal linking, tag strategy for search, and featured snippet targeting.
