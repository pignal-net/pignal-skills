# Article SEO for Blogs

## Title and keySummary Optimization

The `keySummary` field in Pignal becomes the page title and primary heading. It's the single most important SEO element for each post.

### Title Craft Principles

**Be specific over clever.** "5 TypeScript Patterns That Eliminated 80% of Our Runtime Errors" outperforms "TypeScript Tips" because it promises concrete value and includes specificity.

**Front-load the value.** Search results truncate at ~60 characters. Put the most important words first: "React Performance: How We Cut Bundle Size by 60%" not "How We Used Various Techniques to Eventually Cut Our React Bundle Size."

**Include the target keyword naturally.** If someone would search for "PostgreSQL connection pooling," make sure those words appear in your title. But never sacrifice readability for keyword stuffing.

**Match search intent.** "How to" titles attract tutorial seekers. "Why" titles attract understanding seekers. "Best" titles attract comparison shoppers. Choose the format that matches your content's purpose.

## Heading Hierarchy for Search

Search engines use heading structure to understand content organization.

- **H1**: The title (keySummary) — one per page, automatically rendered
- **H2**: Major sections — these often become featured snippet answers
- **H3**: Subsections within H2s
- Never skip levels (H2 → H4) — it signals poor structure

**Write headings as answers.** "How Connection Pooling Works" is more useful than "Background." Headings that answer questions can appear directly in search results.

## Internal Linking Strategy

Internal links help search engines discover content relationships and distribute page authority.

**Link to related posts naturally.** When you mention a concept you've written about before, link to it. This isn't just for SEO — it genuinely helps readers.

**Use descriptive link text.** "Read more about [connection pooling best practices]" is better than "Read more [here]." Link text tells search engines what the target page is about.

**Create topic clusters.** Write one comprehensive "pillar" post, then link multiple related posts to it and vice versa. This signals to search engines that you have depth on the topic.

## Tag Strategy for SEO

Tags in Pignal generate filterable category pages — each one is a potential search landing page.

**Use consistent, keyword-aligned tags.** If your target audience searches for "react hooks," use that as a tag, not "hooks" or "react-hooks-patterns."

**Keep tags broad enough to have multiple posts.** A tag with one post is a dead-end page. Aim for 3+ posts per tag before creating it.

**Don't over-tag.** 2-5 tags per post is the sweet spot. More than that dilutes the signal.

## Featured Snippet Optimization

Featured snippets (position zero in Google) favor structured content:

- **Paragraph snippets**: Answer the question in 40-60 words immediately after the heading
- **List snippets**: Use numbered or bulleted lists under descriptive headings
- **Table snippets**: Use markdown tables for comparison data

**Pattern for snippet targeting:**
```
## What is [Topic]?

[Topic] is [clear, concise 40-60 word definition that directly answers the question].
```

## Visibility Model and SEO

Only `vouched` content in Pignal is publicly visible and SEO-indexed. This is a feature, not a limitation:

- **Draft privately** (`private`) — no risk of indexing incomplete content
- **Share for review** (`unlisted`) — shareable via token, not indexed
- **Publish when ready** (`vouched`) — now visible to search engines

This prevents the common SEO mistake of publishing drafts that get indexed before they're ready, creating a poor first impression with search engines.
