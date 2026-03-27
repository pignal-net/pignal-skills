# LLM Readiness

## The Shift from Search to Synthesis

Traditional search engines index pages and return links. AI assistants read pages and return answers. The difference matters: when an LLM answers a question, it synthesizes information from multiple sources and presents a direct response. The sources that are easiest for the LLM to understand and extract from are the sources that get cited.

This is not a future possibility -- it is happening now. ChatGPT, Claude, Perplexity, and Google's AI Overviews already consume website content to generate answers. Sites that structure their content for machine readability have a structural advantage in this new discovery channel.

## The llms.txt Specification

The llms.txt specification (llmstxt.org) defines a standard way for websites to provide AI-readable summaries of their content and capabilities. It is the AI equivalent of robots.txt -- a machine-readable guide that helps AI systems understand what a site is and how to interact with it.

### How It Works

A website serves a plain text file at `/llms.txt` containing:

1. **Title** -- the site name as a markdown H1
2. **Description** -- a blockquote summarizing the site's purpose
3. **Site information** -- owner, content counts, categories
4. **Access methods** -- URLs for different ways to consume the content (HTML, API, feeds, MCP)

The format is deliberately simple. It uses markdown headings and bullet points -- structures that every LLM can parse reliably.

### llms.txt vs llms-full.txt

The specification supports two levels of detail:

**`/llms.txt`** is the summary. It answers: what is this site, who runs it, what does it contain, and how do I access it. An LLM reading this file can decide whether the site is relevant to its current task.

**`/llms-full.txt`** is the comprehensive guide. It adds: detailed content type descriptions with usage guidance, validation actions, workspace organization, quality guidelines, and MCP tool documentation. An LLM reading this file can interact with the site's content effectively.

### What Pignal Generates

Pignal generates both files automatically from the site's configuration and content.

**`/llms.txt` includes:**
- Site title and description (from settings)
- Owner name
- Published item count and type count
- Content types with descriptions
- Access method table (browse, Atom feed, REST API, MCP, raw markdown, detailed guide)

**`/llms-full.txt` adds:**
- Detailed type descriptions with guidance (when to use, key summary patterns, examples, content tips)
- Validation actions per type
- Workspaces with descriptions
- Quality guidelines and character limits
- MCP tool descriptions with parameter details

Both files are cached for one hour (`Cache-Control: public, max-age=3600`) and served with `Content-Type: text/plain; charset=utf-8`.

## Structuring Content for LLM Consumption

Beyond the llms.txt files, every public page on a Pignal site is potential input for an LLM. The way content is structured affects how reliably an LLM can extract and cite information.

### Explicit Context

LLMs process content in chunks and do not reliably carry context across long passages. Each section should be self-contained enough that a reader (human or AI) dropping into the middle of the page can understand it.

**Weak:** "As mentioned above, this also applies here."

**Strong:** "The 500-row batch limit in Cloudflare D1 applies to both INSERT and UPDATE operations."

The strong version restates the specific fact. An LLM extracting this paragraph does not need to find "above" to understand the claim.

### Definitions at First Use

When a page introduces a domain-specific term, define it immediately. LLMs that encounter an undefined term may misinterpret it or hallucinate a definition.

**Weak:** "Use PKCE for the mobile flow."

**Strong:** "Use PKCE (Proof Key for Code Exchange) -- a security extension to OAuth 2.0 that prevents authorization code interception -- for the mobile flow."

The strong version ensures that any LLM extracting this sentence captures both the term and its meaning.

### One Idea Per Paragraph

LLMs typically process text in paragraph-sized chunks. A paragraph that makes two distinct points risks having one point extracted while the other is lost.

**Weak:** "Connection pooling reduces cold-start latency by reusing existing connections. It also simplifies connection management because the pool handles lifecycle automatically, and you should configure the pool size based on your expected concurrency."

**Strong (split into separate paragraphs):**

"Connection pooling reduces cold-start latency by reusing existing connections instead of establishing new ones for each request."

"Pool size should be configured based on expected concurrency. Too small and requests queue; too large and idle connections waste database resources."

### Headings as Semantic Markers

LLMs use headings to understand content hierarchy, just like search engines do. A heading that says "Background" provides no semantic value. A heading that says "Why Connection Pooling Matters for Serverless Functions" tells the LLM what the section is about before it reads the body.

This is especially important for RAG (Retrieval-Augmented Generation) systems that chunk content by heading. The heading becomes the label for the chunk, and the chunk is retrieved based on how well the label matches the query.

### Structured Formats Over Prose

Tables, lists, and code blocks are easier for LLMs to parse and cite than dense prose.

**Prose:** "The D1 batch limit is 500 rows for inserts and 500 rows for updates, but there is no limit for selects, and the maximum database size is 10GB."

**Table:**

| Operation | Limit |
|-----------|-------|
| INSERT batch | 500 rows |
| UPDATE batch | 500 rows |
| SELECT | No row limit |
| Database size | 10 GB |

The table version is unambiguous. An LLM can extract any single row without misinterpreting adjacent information.

### Code Blocks with Language Tags

Fenced code blocks with language identifiers serve two purposes for LLMs:

1. The language tag tells the LLM what programming language the code is in, preventing misidentification
2. The fenced block clearly delineates executable content from explanatory prose

Always include a language tag. Always include comments in code blocks when the purpose of a line is not obvious.

## Raw Markdown Access

Pignal serves every public item as raw markdown at `/item/:slug.md`. This is the cleanest format for LLM consumption -- no HTML parsing needed, no navigation chrome, no JavaScript, just the content.

LLMs and RAG systems that want to consume Pignal content programmatically should use the `.md` endpoint. It returns the item's markdown content with `Content-Type: text/markdown; charset=utf-8`.

## MCP as an AI Interface

Every Pignal site is an MCP (Model Context Protocol) server at `/mcp`. MCP provides a structured API for AI systems to interact with the site's content -- listing items, searching, creating, and managing content through typed tool calls.

The llms-full.txt file documents available MCP tools, making it possible for an AI system to:

1. Read llms.txt to discover the site
2. Read llms-full.txt to understand its capabilities
3. Connect via MCP to interact with the content programmatically

This three-layer accessibility (HTML for humans, markdown for simple extraction, MCP for full interaction) makes Pignal sites maximally useful to AI systems at every level of sophistication.

## Practical Optimization Checklist

When optimizing a Pignal site for LLM readiness:

- [ ] Verify `/llms.txt` returns a valid response with current site information
- [ ] Verify `/llms-full.txt` includes type descriptions, quality guidelines, and MCP tools
- [ ] Check that site settings include a meaningful `source_title` and `source_description` (these feed into llms.txt)
- [ ] Check that `owner_name` is set (the author attribution that appears in llms.txt)
- [ ] Review content types for clear `description` and `guidance` fields (these appear in llms-full.txt)
- [ ] Ensure vouched items use descriptive headings (not labels) for RAG chunk quality
- [ ] Ensure each section is self-contained with explicit context (no dangling references)
- [ ] Verify domain-specific terms are defined at first use
- [ ] Check that tables and lists are used where structured information is conveyed
- [ ] Confirm code blocks include language tags

## The Compound Effect

A Pignal site that is optimized for both traditional SEO and LLM readiness gets discovered through two channels simultaneously. Search engines index the structured data and serve rich results. AI systems read the llms.txt, consume the markdown, and cite the content in answers.

These channels reinforce each other. A page that ranks well in search results gets more traffic, which signals quality to AI training pipelines. A page that AI systems cite frequently gets more inbound links, which improves search rankings. The sites that invest in both discovery channels now will benefit from this compound effect as AI-assisted search grows.
