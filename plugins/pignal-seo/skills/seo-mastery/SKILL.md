# SEO Mastery

Cross-template SEO optimization for any Pignal site. On-page SEO principles, structured data, E-E-A-T signals, LLM readiness, and content auditing. Use when auditing SEO, optimizing content for search, checking structured data, improving site visibility, or asking about SEO best practices. Always use this skill when SEO, search ranking, or discoverability is mentioned.

---

## The MCP Workflow

Every SEO operation follows this sequence. Do not skip steps -- each one prevents a different class of mistake.

1. **Discover** -- `get_site_tools` to learn what the site can do.
2. **Learn the site** -- `call_site_tool` with `get_metadata` to get types, workspaces, quality guidelines, and validation limits. SEO optimization requires knowing what the site contains and how it is configured.
3. **Inventory** -- `call_site_tool` with `list_items` to see existing content. Filter by type or workspace to understand the site's content profile.
4. **Audit** -- Apply the SEO principles below to assess title quality, structure, metadata completeness, and discoverability signals.
5. **Optimize** -- Use `update_item` to improve keySummaries, restructure content, fix tags, or adjust metadata. Use `validate_item` to strengthen E-E-A-T signals.
6. **Publish** -- Use `vouch_item` with SEO-optimized slugs to make content publicly indexed.
7. **Report** -- Summarize findings: what was improved, what is still weak, and what the next priorities are.

---

## Why SEO Matters for Pignal Sites

Search engines are the primary discovery mechanism for most websites. A Pignal site that follows SEO principles compounds in value over time -- every vouched item becomes a permanent, indexable asset that attracts visitors for months or years after publication.

Pignal's architecture provides strong SEO foundations out of the box: clean URLs (`/item/:slug`), JSON-LD structured data, sitemap generation, Open Graph tags, RSS/Atom feeds, and `llms.txt` for AI readability. The skill's job is to ensure that the content itself maximizes the value of these foundations.

SEO is not a separate step bolted onto content creation. It is a way of thinking about content -- asking "what would someone search for to find this?" before writing, not after.

---

## The keySummary as Title Tag

The `keySummary` field becomes the page's `<title>` tag and the `<h1>` heading. It is the single most important SEO element for each item because it appears in three critical places: the browser tab, the search engine results page (SERP), and the site's content feed.

### Title Craft Principles

**Front-load the value.** Search results truncate titles at approximately 60 characters. The most important words must appear first. "React Server Components: Eliminating Client-Server Waterfalls" works because the topic appears immediately. "How I Eventually Learned About React Server Components and Their Benefits" buries the signal.

Why this matters: Google bolds matching search terms in the title. If those terms are truncated, the result looks less relevant to the searcher -- even if the content is exactly what they need.

**Be specific over clever.** A title that names the concrete topic outperforms one that teases. "Fixed CORS Errors by Moving Headers to the Worker" communicates the problem and the solution. "The CORS Mistake Everyone Makes" communicates nothing except a promise to reveal something later.

Why this matters: Specificity increases click-through rate (CTR) because searchers can tell from the SERP whether the page answers their question. Higher CTR signals relevance to search engines, creating a positive feedback loop.

**Include the target keyword naturally.** If someone would search for "Cloudflare D1 migrations," those words should appear in the title. But never sacrifice readability. "Cloudflare D1 Migrations: Step-by-Step Guide for Production" is natural. "Cloudflare D1 Migrations D1 Database Migration Guide Cloudflare" is keyword stuffing and will be penalized.

**Match search intent.** The title format should match what the searcher expects:
- "How to..." titles attract people seeking instructions
- "Why..." titles attract people seeking understanding
- "[X] vs [Y]" titles attract people comparing options
- "Best..." or "[N] Ways to..." titles attract people browsing options

Choose the format that matches the content's purpose, not the format that sounds most clickable.

### Title Anti-Patterns

| Problem | Example | Why It Fails |
|---------|---------|-------------|
| Too vague | "Database thoughts" | No search query matches this |
| Too long | 80+ characters | Truncated in SERPs, value hidden |
| Clickbait | "You won't believe what happened" | Breaks trust, high bounce rate |
| Keyword stuffed | "React React Hooks React Tutorial" | Penalized by search engines |
| Repeats first line | keySummary identical to opening sentence | Wasted opportunity for different signal |

---

## Heading Hierarchy and Content Structure

Search engines use heading structure to understand content organization and extract featured snippets. Readers use headings to scan. Both purposes are served by the same principle: headings should be complete thoughts, not labels.

### The Hierarchy Rule

- **H1**: The keySummary -- one per page, rendered automatically by Pignal
- **H2** (`##`): Major sections. These are the primary structural markers and the most common source of featured snippets.
- **H3** (`###`): Subsections within H2s. Use when a section has distinct sub-topics.
- Never skip levels (H2 directly to H4). Skipping signals poor structure to crawlers and accessibility tools.

### Headings as Answers

A heading like "Background" tells the reader nothing. A heading like "Why Connection Pooling Matters for Edge Deployments" tells the reader exactly what value the section provides. Search engines can extract the heading-plus-first-paragraph as a featured snippet when the heading phrases a question and the text answers it.

**Pattern for featured snippets:**

```
## What is [topic]?

[Topic] is [40-60 word definition that directly answers the question].
```

This structure targets paragraph snippets -- the most common type. List snippets are targeted by placing a numbered or bulleted list immediately after a descriptive heading.

### Content Structure for Scannability

SEO and readability are not in tension. Content that is easy to scan is also content that search engines can parse effectively.

**Short paragraphs.** Each paragraph should make one point in 2-4 sentences. Long paragraphs get skipped by readers and dilute keyword density for crawlers.

**Lists for parallel items.** Bullet lists signal to search engines that the page contains structured information. Google frequently extracts lists as featured snippets.

**Tables for comparisons.** Markdown tables are rendered as HTML `<table>` elements, which Google can extract as table snippets. A comparison table is one of the highest-value content structures for SEO.

**Code blocks with language tags.** Fenced code blocks with language identifiers help search engines understand that the page contains technical content, improving relevance for programming queries.

---

## E-E-A-T and Pignal's Validation System

E-E-A-T stands for Experience, Expertise, Authoritativeness, and Trustworthiness. It is Google's framework for evaluating content quality. Pignal's architecture directly supports all four signals.

### Experience

First-person framing in content signals lived experience. When an item says "I discovered that PKCE is required for mobile OAuth," it encodes a specific experience that is verifiable against the author's background. This is more valuable to both readers and search engines than an unsourced claim.

Pignal tracks the `sourceAi` field on every item, recording whether content was created via MCP and which client was used. This transparency about AI assistance is itself an experience signal -- it shows the author is using tools, not hiding them.

### Expertise

Pignal's type system naturally demonstrates expertise. A site with well-organized content types (insights, decisions, solutions, references) shows systematic thinking. Type-specific validation actions (e.g., "Technically Verified," "Peer Reviewed") signal that domain knowledge was applied during review.

The `get_metadata` response reveals the site's quality guidelines and validation actions. Use these to understand what level of expertise the site owner expects.

### Authoritativeness

Authority comes from identity and consistency. Pignal's author attribution chain ensures every public page shows a real author:

1. `owner_name` (from settings) -- the preferred attribution
2. `source_title` -- the site name as fallback
3. Domain hostname -- the last resort

This chain means no public page ever shows "Anonymous." The JSON-LD `author` field always resolves to a named person with optional `sameAs` links (GitHub profile), which search engines use to build author entity graphs.

### Trustworthiness

Pignal's validation system is the primary trustworthiness signal. When you call `validate_item`, it:

1. Records the validation action label (e.g., "Confirmed," "Fact-Checked")
2. Renders this label publicly on the item page alongside the author name
3. Includes `creativeWorkStatus` and `editorialNote` in the JSON-LD structured data

This is not a rubber stamp. Choosing "Confirmed" when you have not verified the claims degrades trust. Choose the validation action that honestly reflects the assessment performed.

### The Validation Workflow for SEO

Before vouching any item:

1. Check `get_metadata` for available validation actions per type
2. Evaluate whether the content meets the action's standard
3. Apply via `validate_item` with the appropriate action
4. Then vouch with `vouch_item` to publish

Items validated before vouching carry stronger E-E-A-T signals from the moment they are indexed.

---

## Structured Data (JSON-LD)

Pignal automatically generates JSON-LD structured data for every public page. Understanding what gets generated helps you optimize the content that feeds into it.

### What Pignal Generates

**Source page** (the homepage `/`): A `Blog` (or template-specific type like `Store`, `WebSite`) JSON-LD object with `name`, `description`, `url`, and `author`.

**Item pages** (`/item/:slug`): A `BlogPosting` (or template-specific type like `Product`, `Article`) JSON-LD object with:
- `headline` (or `name` for Products) -- from `keySummary`
- `description` -- first 160 characters of stripped markdown content
- `author` -- Person with name and optional `sameAs` (GitHub URL)
- `datePublished` -- `vouchedAt` or `createdAt`
- `dateModified` -- `updatedAt`
- `articleSection` -- the item's type name
- `keywords` -- the item's tags, comma-separated
- `publisher` -- Organization (the site)
- `mainEntityOfPage` -- canonical URL

### How to Optimize for Structured Data

The content you control directly maps to structured data fields:

| What You Write | Where It Appears in JSON-LD |
|---|---|
| `keySummary` | `headline` (title in rich results) |
| First 160 chars of content | `description` |
| Tags | `keywords` |
| Content type name | `articleSection` |
| Validation action | `creativeWorkStatus` / `editorialNote` |
| Vouch timestamp | `datePublished` |

**Actionable principles:**
- Write the first 160 characters of content as if they were a meta description. They literally become one.
- Use tags that match real search queries, not internal jargon.
- Validate items before vouching so the structured data includes trust signals from the moment of indexing.
- Keep content updated -- `dateModified` freshness is a ranking signal.

---

## LLM Readiness

Pignal serves `llms.txt` and `llms-full.txt` at the root of every site, following the llmstxt.org specification. These files help AI systems understand what the site is, what it contains, and how to access its content.

### Why LLM Readiness Matters

AI assistants increasingly answer questions by synthesizing information from websites. A site that is easy for LLMs to understand and cite gets referenced more often. This is the search engine of tomorrow -- and the sites that prepare now will have a structural advantage.

### What Pignal Generates

**`/llms.txt`** -- A concise summary containing:
- Site title and description
- Owner name
- Count of published items and types
- Content types with descriptions
- Access methods table (HTML, Atom feed, REST API, MCP, raw markdown)

**`/llms-full.txt`** -- A comprehensive guide containing everything in `llms.txt` plus:
- Detailed type descriptions with guidance (when to use, key summary patterns, examples, content tips)
- Validation actions per type
- Workspaces with descriptions
- Quality guidelines and character limits
- MCP tool descriptions

### How to Optimize for LLM Readability

LLMs consume the `llms.txt` files, but they also consume the item pages themselves (both HTML and `/item/:slug.md` raw markdown). Optimize for both:

**Structured content wins.** LLMs extract information more reliably from headings, lists, and tables than from dense prose. The same structure that helps human readers also helps AI readers.

**Explicit context beats implied context.** "In Cloudflare Workers, D1 batch operations have a 500-row limit" is extractable. "The limit I mentioned earlier also applies here" is not.

**Definitions and explanations.** When using domain-specific terms, define them on first use. LLMs that encounter an unfamiliar term may misinterpret the surrounding content.

**One idea per paragraph.** LLMs typically process content in chunks. A paragraph that mixes two ideas may cause one to be lost in the chunking process.

---

## Tag Strategy for SEO

Tags in Pignal generate filterable views on the source page. Each tag filter is a URL parameter (`?tag=react`) that creates a distinct content grouping. While these are not separate indexable pages, they influence discoverability through the site's internal structure and through the JSON-LD `keywords` field.

### Tag Principles for Search

**Align tags with search queries.** Think about what someone would type into a search engine. If your audience searches for "react hooks," use `react-hooks` as a tag, not `hooks` or `react-hooks-advanced-patterns`.

**Reuse existing tags aggressively.** Before creating a new tag, check `list_items` to see which tags already exist. A tag used across 5 items creates a meaningful content cluster. A tag used once creates nothing.

**Keep tags to 2-5 per item.** Fewer than 2 means the item is under-categorized and will not appear in filtered views. More than 5 dilutes the signal -- each additional tag adds less value than the last.

**Use lowercase, single-concept tags.** Tags are normalized to lowercase by Pignal. Keep them simple: `performance`, `typescript`, `deployment`. Avoid compound tags like `advanced-react-performance-tips`.

### Tags and JSON-LD Keywords

Every tag appears in the JSON-LD `keywords` field as a comma-separated string. Search engines use keywords as a relevance signal. This means tag quality directly affects structured data quality.

---

## Visibility Model and SEO

Pignal's three-tier visibility model is an SEO feature, not a limitation.

**`private`** -- Content in draft. Not accessible to anyone except via API/MCP. No risk of premature indexing.

**`unlisted`** -- Accessible via a share token URL (`/s/:token`). Useful for pre-publication review. Share token URLs are excluded from sitemaps and have `noindex` meta tags, so search engines will not index incomplete content.

**`vouched`** -- Public, listed on the source page, included in the sitemap, and fully SEO-indexed with a permanent slug at `/item/:slug`.

Why this matters: One of the most common SEO mistakes is publishing drafts that get indexed before they are ready. Search engines form an impression of a page on first crawl. A half-finished article that gets indexed creates a poor first impression that takes time to recover from -- even after the content is completed.

The private-to-vouched progression ensures that content is only indexed when it meets your quality bar.

### Slug Optimization

When vouching with `vouch_item`, you set the URL slug. This slug is permanent -- changing it later breaks existing links and loses accumulated SEO equity.

**Good slugs:**
- Lowercase and hyphenated: `react-server-components-data-fetching`
- Descriptive: someone should understand the topic from the URL
- Concise: 3-6 words. Shorter URLs perform better in SERPs.
- No dates: URLs should be permanent. Dates make them look stale.
- No stop words: skip "the," "and," "a" unless they are part of a proper name

**Bad slugs:**
- `post-1`, `my-article` -- no topic signal
- `2024-03-15-react-thoughts` -- date will age poorly
- `how-to-use-the-new-react-server-components-for-data-fetching-in-next-js` -- too long, truncated in SERPs

---

## Open Graph and Social Sharing

Pignal generates Open Graph meta tags for every public page:

- `og:type` -- `article` for item pages, `website` for the source page
- `og:title` -- the keySummary (item) or source title
- `og:description` -- first 160 characters of stripped content
- `og:url` -- canonical URL
- `og:image` -- resolved from settings or GitHub avatar
- Twitter Card tags (mirrors Open Graph)

### Optimizing for Social Sharing

The `og:description` is the first 160 characters of the content body (after stripping markdown and directives). This means the opening of your content doubles as the social sharing preview.

Write the first paragraph as if it will be read in isolation -- because on social media, it will be. A strong opening that names the problem and hints at the solution creates a compelling share preview without any extra work.

The `og:image` resolves through a chain: custom OG image URL (from settings) > GitHub avatar > default. Setting a custom `source_og_image_url` in the site settings gives you control over the visual that appears in social shares.

---

## MCP SEO Audit Workflow

When auditing a site's SEO posture via MCP, follow this systematic approach:

### Step 1: Understand the Site

```
call_site_tool(siteId, "get_metadata")
```

Review: types, workspaces, quality guidelines, validation limits. Note the keySummary character limits -- titles exceeding 60 characters will be truncated in SERPs.

### Step 2: Inventory Published Content

```
call_site_tool(siteId, "list_items", { limit: 100 })
```

For each item, assess:
- **keySummary quality**: Is it specific, front-loaded, keyword-rich, and under 60 characters?
- **Tag quality**: Are tags keyword-aligned, reused across items, and limited to 2-5?
- **Validation status**: Has it been validated? Items without validation lack E-E-A-T trust signals.
- **Content opening**: Do the first 160 characters serve as an effective meta description?

### Step 3: Identify Issues

Categorize findings into:
- **Quick wins** -- keySummary rewrites, tag cleanup, missing validations. High impact, low effort.
- **Content gaps** -- Topics the audience searches for but the site does not cover.
- **Structural issues** -- Heading hierarchy violations, wall-of-text content, missing code block language tags.

### Step 4: Fix and Optimize

For each item needing improvement:

```
call_site_tool(siteId, "update_item", {
  itemId: "...",
  keySummary: "Improved, keyword-rich title under 60 chars",
  tags: ["aligned", "keyword", "tags"]
})
```

For items missing validation:

```
call_site_tool(siteId, "validate_item", { itemId: "...", actionId: "..." })
```

### Step 5: Report

Summarize: how many items audited, how many improved, what the top remaining priorities are, and what content gaps exist.

---

## Internal Linking

Internal links help search engines discover content relationships and distribute page authority across the site. Pignal items can reference other items by mentioning titles or linking to `/item/:slug` URLs.

**Link naturally.** When content references a concept covered in another item, link to it. This helps both readers and crawlers.

**Use descriptive link text.** "Read more about [connection pooling in edge deployments](/item/connection-pooling-edge)" is better than "[click here](/item/connection-pooling-edge)." Link text tells search engines what the target page is about.

**Create topic clusters.** Write one comprehensive item as a "pillar" covering a broad topic, then write several focused items that link back to it. This signals depth on the topic.

---

## RSS/Atom and Syndication

Pignal serves an Atom feed at `/feed.xml` containing the 50 most recent vouched items. The feed includes titles, content, and metadata -- everything a feed reader or aggregator needs.

The feed URL is also included as a `<link rel="alternate">` in the HTML head of every page, making it auto-discoverable by feed readers and search engines.

Why this matters for SEO: RSS/Atom feeds accelerate content discovery. When a new item is vouched, it appears in the feed immediately. Feed readers, aggregators, and services that monitor feeds will discover the content faster than waiting for search engine crawls.

---

## Sitemap

Pignal generates XML sitemaps automatically at `/sitemap.xml`. For sites with many items, paginated sub-sitemaps are generated (`/sitemap-1.xml`, `/sitemap-2.xml`, etc.) with a sitemap index at the root URL.

The sitemap is referenced in `robots.txt` and includes:
- The homepage with `priority: 1.0` and `changefreq: daily`
- Every vouched item with `priority: 0.8`, `changefreq: monthly`, and `lastmod` from the item's `updatedAt` timestamp

No manual sitemap management is needed. Vouching an item automatically includes it in the next sitemap generation.

---

## Common SEO Mistakes on Pignal Sites

| Mistake | Why It Hurts | Fix |
|---------|-------------|-----|
| Publishing without validating | Missing E-E-A-T trust signals in JSON-LD | Always `validate_item` before `vouch_item` |
| Vague keySummaries | No search query matches, low CTR | Rewrite with specific, keyword-rich titles |
| Over-tagging (8+ tags) | Diluted keyword signal, no meaningful clusters | Reduce to 2-5 high-value tags |
| Single-use tags | Dead-end filtered views | Reuse existing tags or remove |
| Poor slug choices | Bad URL signal, wasted permanent URL | Choose descriptive, concise slugs at vouch time |
| Weak opening paragraphs | Poor meta description, poor social preview | Write the first 160 chars as a standalone pitch |
| Skipping heading levels | Signals poor structure to crawlers | Follow H1 > H2 > H3 hierarchy strictly |
| Stale content | Outdated `dateModified`, declining relevance | Regular audits, update and re-validate |
| No internal links | Isolated pages, no authority distribution | Reference related items with descriptive link text |

---

## References

For deeper exploration of specific topics:

- **[On-Page SEO](references/on-page-seo.md)** -- Title tags, meta descriptions, heading structure, image alt text, and URL structure fundamentals.
- **[Structured Data Principles](references/structured-data-principles.md)** -- JSON-LD fundamentals, schema.org vocabulary, required vs recommended properties, testing, and rich result eligibility.
- **[LLM Readiness](references/llm-readiness.md)** -- The llms.txt specification, AI content accessibility, and structuring content for LLM consumption.
