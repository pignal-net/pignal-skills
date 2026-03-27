# Technical Documentation Craft

Expert technical documentation writing, information architecture, and knowledge base management for wiki and documentation sites. Use this skill whenever creating documentation, organizing knowledge bases, writing reference material, how-to guides, tutorials, or explanations — or when operating a Pignal wiki site. Covers the full lifecycle: planning information architecture, choosing document types, drafting with the right methodology, cross-referencing, validating, and publishing.

---

## The MCP Workflow

Every documentation operation follows this sequence. Do not skip steps — each one prevents a different class of mistake.

1. **Discover** — `get_site_tools` to learn what the site can do.
2. **Learn the site** — `call_site_tool` with `get_metadata` to get the site's types, workspaces, limits, and quality guidelines. This is not optional. Types, workspaces, and field limits vary per site and are configured by the site owner. Never assume them.
3. **Plan** — Decide document type, workspace placement, tags, and structure based on what `get_metadata` returned. Map the *purpose of the document* to a type (see "Choosing the Right Document Type" below).
4. **Draft** — Write the keySummary and markdown content following the craft principles in this skill.
5. **Create** — `call_site_tool` with `save_item` to save the document.
6. **Validate** — `call_site_tool` with `validate_item` to apply a quality action (e.g., Reviewed, Verified). Documentation demands accuracy — validation is not a formality.
7. **Publish** — `call_site_tool` with `vouch_item` to set visibility to `vouched` with a clean URL slug.

---

## The Diataxis Framework

Documentation is not a single genre. It is four genres disguised as one word. The Diataxis framework identifies them by crossing two axes: what the reader is *doing* (learning vs working) and what they need (practical steps vs theoretical understanding).

### Tutorials — Learning-Oriented

Tutorials teach by doing. The reader follows along step by step, and the outcome is *not knowledge of the subject* but *confidence that they can engage with it*. A tutorial about deploying a Cloudflare Worker does not need to explain how V8 isolates work — it needs to get the reader from zero to a running worker.

**Why tutorials are hard to write well:** The author already knows the subject. The curse of knowledge makes it difficult to identify which steps feel obvious to experts but are invisible to beginners. Every time you write "simply" or "just," you are papering over a step the reader may not understand.

**Principles:**
- Start from a known state. "You have Node.js 18+ installed and a Cloudflare account" is a valid starting point. "You know JavaScript" is too vague.
- Every step must produce a visible result. If the reader cannot verify that a step worked, they cannot trust the next step.
- Do not explain *why* in a tutorial. Explanation belongs in explanation documents. Tutorials that pause to teach theory lose momentum and confuse two different reading modes.
- Use second person ("you"). Tutorials are conversations where the author guides the reader through an experience.

### How-To Guides — Task-Oriented

How-to guides solve specific problems. The reader arrives with a goal ("I need to migrate my database") and leaves with that goal accomplished. Unlike tutorials, how-to guides assume the reader already has context — they know what they want to do, they just need the steps.

**The key distinction from tutorials:** Tutorials choose the destination. How-to guides assume the reader already chose it. A tutorial says "Let's build a blog." A how-to guide says "How to add RSS feeds to your existing blog."

**Principles:**
- Name the goal in the title. "How to configure CORS for Cloudflare Workers" tells the reader immediately whether this document solves their problem.
- List prerequisites up front. Do not bury assumptions three steps deep.
- Stay focused on the task. If the reader needs background, link to an explanation document rather than embedding it.
- Provide the minimum viable path first, then optional variations. Not everyone needs the advanced configuration.

### Reference — Information-Oriented

Reference documentation describes the system as it is. API endpoints, configuration options, data schemas, command flags — anything a reader looks up rather than reads through. Reference material is consulted, not studied.

**Why reference material is the most neglected:** It feels mechanical to write. There is no narrative arc, no clever insight. But reference documentation is often the most-visited documentation on a site because working developers return to it repeatedly.

**Principles:**
- Structure mirrors the system. If the API has endpoints grouped by resource, the reference documentation has sections grouped by resource. If a config file has sections, the reference follows those sections.
- Be exhaustive within scope. A reference document that covers 80% of the options forces the reader to find the remaining 20% somewhere else — which means they stop trusting the reference.
- Use consistent formatting. Every API endpoint should have the same sections (method, path, parameters, response, errors). Tables are the natural format for reference data.
- Do not explain *why* things work the way they do. Reference answers "what" and "how." Explanation answers "why."

### Explanation — Understanding-Oriented

Explanation documents help the reader understand concepts, architecture, design decisions, and the reasoning behind systems. They answer "why" questions. Why does this system use eventual consistency? Why are permissions structured this way? Why was this library chosen over alternatives?

**The trap of explanation:** It is tempting to blend explanation into every other document type. Resist this. A how-to guide that pauses for three paragraphs of architectural context loses the reader who just wants to complete the task. Link to the explanation instead.

**Principles:**
- Multiple perspectives strengthen explanation. Show the same concept from different angles — analogy, comparison, historical context, tradeoff analysis.
- Acknowledge alternatives. "We use SQLite because..." is stronger when followed by "We considered PostgreSQL but..."
- Explanation can be opinionated. Unlike reference material, explanation documents benefit from the author's perspective on *why* things should be a certain way.
- Connect to other document types. Explanation provides the "why" that tutorials and how-to guides deliberately omit.

### Choosing the Right Document Type

The same subject can appear in all four types. "Authentication" could be a tutorial ("Build a login flow from scratch"), a how-to guide ("How to add OAuth to an existing app"), reference ("Authentication API endpoints"), or explanation ("Why we use JWTs instead of sessions").

**Selection by reader intent:**
- Reader wants to learn → Tutorial
- Reader wants to accomplish → How-to guide
- Reader wants to look up → Reference
- Reader wants to understand → Explanation

Always read the `guidance` field from `get_metadata` for each type. Site owners customize patterns and hints to match their domain. Follow them.

---

## Writing for Scanners

Documentation readers are not book readers. They arrive with a question, scan for the answer, and leave. Your job is to make scanning productive.

### Headings as Entry Points

Every heading should work as a standalone statement that helps the scanner decide whether to read the section. "Error Handling" is a label. "How the system handles validation errors" is a heading that tells the scanner what they will learn.

Why this matters: Search engines and documentation site navigation use headings to build tables of contents and search indexes. A heading that communicates its section's content improves both human navigation and machine discoverability.

### The Inverted Pyramid

Put the most important information first — in the document, in each section, and in each paragraph. Journalists call this the inverted pyramid. It works for documentation because readers who find their answer in the first sentence do not need to read further, and readers who need more depth continue naturally.

**At document level:** Start with what the thing *is* and what it *does*, not with its history or architecture.

**At section level:** Lead with the actionable information, follow with the details.

**At paragraph level:** First sentence states the point. Subsequent sentences support it. If you need to read three sentences before the point becomes clear, the paragraph is structured backwards.

### Structural Patterns for Density

**Tables over paragraphs for comparisons.** A table comparing three database options with columns for latency, cost, and operational complexity communicates in 10 seconds what three paragraphs communicate in 2 minutes.

**Definition lists for terminology.** When introducing domain-specific terms, use a consistent format (term followed by definition) rather than burying definitions in prose.

**Callout blocks for critical information.** Warnings, prerequisites, and breaking changes should be visually distinct from body text. Use blockquotes or bold prefixes to signal importance.

**Short paragraphs.** Documentation paragraphs should rarely exceed 3-4 sentences. Each paragraph should make one point. If a paragraph covers two points, split it.

---

## Code Examples in Documentation

Code examples are the highest-density teaching tool in technical documentation. A good code example replaces paragraphs of explanation. A bad one creates more confusion than no example at all.

### Principles of Good Code Examples

**Every example must run.** If you include a code block, the reader should be able to copy-paste it and get the expected result. Broken examples destroy trust faster than anything else in documentation.

**Show input and output together.** When demonstrating a command or function, include what the reader types AND what they should see. This serves as both instruction and verification.

**Use realistic values, not placeholders.** `user_id: "abc123"` is better than `user_id: "YOUR_USER_ID_HERE"` because it shows the *shape* of a real value. When the reader must substitute their own value, mark it explicitly and explain what to substitute.

**Progressive complexity.** Start with the simplest working example, then add complexity. Do not show the 15-argument function call first. Show the 2-argument version, prove it works, then introduce optional parameters.

**Language tags on every code block.** Always specify the language for syntax highlighting. Use `bash` for shell commands, `json` for API responses, `sql` for queries. Untagged code blocks render as plain text and lose semantic meaning.

### When to Include Code

- **Always** in how-to guides — they are step-by-step, and steps often involve commands or code.
- **Always** in reference documentation — API examples, config snippets, and expected responses.
- **Selectively** in tutorials — every step that involves the reader doing something should have the exact code.
- **Sparingly** in explanation — only when the code *is* the concept being explained (e.g., showing a design pattern).

---

## Information Architecture

How documents are organized matters as much as how they are written. A well-written document in the wrong place is invisible.

### Workspace as Category

Pignal's workspace system maps naturally to documentation categories. Each workspace becomes a section of the knowledge base. Plan workspace structure before writing individual documents.

**Flat is better than deep.** Three to seven top-level workspaces that each contain 5-30 documents is navigable. Three levels of nested categories with 2 documents each is a maze.

**Name workspaces for the reader's mental model.** "Getting Started," "API Reference," "Architecture" work because they match how developers think about documentation. "Miscellaneous" and "Other" are organizational failures.

### Tagging for Cross-Cutting Concerns

Tags handle topics that span multiple workspaces. A document about "authentication" might live in the "API Reference" workspace but also needs the `security` tag so it appears alongside other security-related documents.

**Tag design principles:**
- Use lowercase, single-concept tags: `authentication`, `deployment`, `migration`
- Aim for 2-5 tags per document
- Reuse existing tags aggressively — check `list_items` or `search_items` before creating new ones
- Avoid meta-tags like `documentation` or `wiki` — every item on the site is already a document

### Progressive Disclosure

Organize content so readers encounter information in order of importance and likelihood of need. The most commonly needed documents (quickstart, API reference, core concepts) should be the easiest to find. Advanced configuration, edge cases, and internal architecture come later.

---

## Cross-Referencing

Documentation pages do not exist in isolation. Each document gains value from its connections to related documents. Effective cross-referencing transforms a collection of pages into a navigable knowledge base.

**When to cross-reference:** Whenever a document mentions a concept that is explained in more depth elsewhere. The reader should never have to search for related information — the document should point them to it.

**Link text should describe the destination.** "See the authentication guide" tells the reader what they will find. "See here" tells them nothing.

**Prefer forward references to backward.** "This topic is covered in depth in the Architecture Explanation" (pointing to another document) is more useful than "As mentioned in the previous section" (forcing the reader to scroll back).

For a deeper treatment of cross-referencing patterns and practices, see the [Cross-Referencing reference](references/cross-referencing.md).

---

## The keySummary

The keySummary is the document's title — the first thing readers see in the feed, the primary text search engines index, and the label used in navigation. In documentation, the keySummary must communicate both the topic and the document type.

**Good keySummary patterns by Diataxis type:**
- Tutorial: "Tutorial: Build a REST API with Hono and D1"
- How-to: "How to configure custom domains for Workers"
- Reference: "CLI command reference"
- Explanation: "Why Pignal uses SQLite at the edge"

**keySummary anti-patterns:**
- Vague: "Database stuff" (no signal about what aspect or document type)
- Too clever: "The Care and Feeding of Your Schema" (cute but unsearchable)
- Too long: Keep within the site's character limits (check `get_metadata`)
- Duplicate: Every keySummary on the site should be unique — duplicates confuse navigation and search

---

## Versioning and Deprecation

Documentation decays. APIs change, configurations evolve, features get deprecated. Managing this decay is part of documentation craft.

**Version-stamp time-sensitive content.** "As of v2.3.0, the `--legacy` flag is required" tells the reader exactly when this information was accurate. Unstamped content forces the reader to guess whether it is current.

**Deprecation is a process, not an event.** When deprecating a document or the feature it describes:
1. Add a prominent notice at the top stating what is deprecated and what replaces it
2. Link to the replacement document
3. Keep the deprecated document published — removing it breaks bookmarks and search results
4. After a reasonable period, update the keySummary to include "[Deprecated]"

**Review cadence.** Schedule periodic reviews of published documentation. Use `list_items` filtered by workspace to audit sections systematically. Outdated documentation is worse than no documentation because it teaches the reader wrong things with authority.

---

## Docs-as-Code Methodology

Documentation should follow the same discipline as code: version-controlled, reviewed, tested, and maintained. This is not a metaphor — it is a methodology.

**Why docs-as-code matters:** When documentation lives outside the development workflow, it drifts out of sync with the system it describes. Treating docs as code means they get the same attention, the same review process, and the same standards as the codebase.

For a detailed exploration of docs-as-code practices, see the [Docs-as-Code reference](references/docs-as-code.md).

---

## Publishing Workflow

After creating a document with `save_item`, it starts as `private`. Move through the quality gates:

1. **Validate** — Use `validate_item` to apply a quality action. For documentation, validation means checking technical accuracy, not just grammar. Choose the action that honestly reflects the document's verification status.
2. **Review the slug** — When publishing with `vouch_item`, provide a clean URL slug. Good documentation slugs describe the content: `configure-custom-domains`, `authentication-api-reference`, `why-sqlite-at-the-edge`. Avoid dates in slugs — documentation URLs should be permanent.
3. **Publish** — Set visibility to `vouched` to make the document public, listed in the wiki, and indexed by search engines.

For batch publishing (e.g., launching an entire documentation section), use `batch_vouch_items` to publish multiple documents at once.

---

## References

For deeper exploration of specific topics:

- **[Docs-as-Code](references/docs-as-code.md)** — Version-controlled documentation, markdown authoring, review processes, testing docs, and keeping documentation in sync with the systems it describes.
- **[Information Architecture](references/information-architecture.md)** — Topic/task/reference organization, navigation design, taxonomy, search optimization, and structuring knowledge for discoverability.
- **[Cross-Referencing](references/cross-referencing.md)** — When to link, link text practices, knowledge graph thinking, avoiding link rot, and building navigable documentation networks.
