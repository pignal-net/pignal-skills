# On-Page SEO

## Title Tags

The title tag is the single most important on-page SEO element. It appears in three places: the browser tab, the search engine results page (SERP), and social media link previews. Every character matters.

### Length

Search engines display approximately 50-60 characters of a title tag before truncating with an ellipsis. Google measures by pixel width, not character count, but 60 characters is a reliable heuristic. Titles longer than this lose their most important words if those words appear at the end.

The practical rule: put the value in the first 50 characters. Everything after that is bonus -- nice to have, but not guaranteed to be seen.

### Structure

Effective title structures follow predictable patterns:

**Topic-first.** "Cloudflare D1 Migrations: A Production Guide" puts the target keyword first. A searcher looking for "Cloudflare D1 migrations" sees matching terms immediately, and Google bolds them.

**Problem-solution.** "Fixed CORS Errors by Moving Headers to the Worker" names both the problem (CORS errors) and the resolution. This structure works well for solution-oriented content because it matches how people search for fixes.

**Comparison.** "SQLite vs PostgreSQL for Edge Deployments" names both options and the context. This matches "X vs Y" search queries, which have high commercial intent.

**How-to.** "How to Configure Workers KV for Session Storage" matches instructional search intent directly.

### Common Mistakes

**Keyword stuffing.** "React Hooks React Tutorial React Guide" reads as spam to both humans and search engines. Use the target keyword once, naturally.

**Vague titles.** "Some Thoughts on Databases" tells search engines nothing about the content. No search query matches this because no one searches for "some thoughts."

**Duplicate titles across pages.** If multiple items on a site have similar titles, search engines cannot distinguish between them and may choose to index only one. Each title should be unique.

**Brand-first format.** "MySite | How to Deploy Workers" wastes the most valuable characters on the site name, which adds no search value. The site name is automatically appended by Pignal in the format "keySummary | Source Title."

## Meta Descriptions

The meta description appears below the title in SERPs. It does not directly affect rankings, but it affects click-through rate (CTR), which indirectly affects rankings. A compelling description converts an impression into a click.

In Pignal, the meta description is automatically generated from the first 160 characters of the content body (after stripping markdown formatting and directives). This means the opening of your content is your meta description.

### Writing Effective Descriptions

**Answer the searcher's question.** If someone searches for "how to fix CORS errors in Workers," the description should confirm that this page answers that question: "CORS errors in Cloudflare Workers happen when response headers are set on the origin instead of the Worker. Here's the fix."

**Include the target keyword.** Google bolds search terms in the description, making the result visually prominent. If the keyword appears naturally in your opening sentences, the description handles itself.

**Create urgency or promise value.** "I spent 3 hours debugging this so you don't have to" is more compelling than "This article discusses CORS configuration." The first promises saved time; the second promises... an article.

**Stay under 160 characters.** Descriptions longer than this get truncated. Since Pignal uses the first 160 content characters, ensure your opening paragraph is tight and complete within this limit.

### When Google Rewrites Descriptions

Google sometimes ignores the provided description and generates its own from page content. This happens when:
- The provided description does not match the search query well
- The description is too generic
- Another section of the page better answers the query

You cannot prevent this, but a well-written opening paragraph that directly addresses the page's topic reduces how often it happens.

## Heading Structure

### The Hierarchy

HTML heading elements (H1-H6) create a document outline that search engines use to understand content organization.

- **H1**: One per page. In Pignal, the `keySummary` is rendered as the H1 automatically.
- **H2**: Major sections. Each H2 starts a new topic within the page. These are the most common source of featured snippets.
- **H3**: Subsections within an H2. Use when a section has distinct sub-topics that benefit from their own headings.
- **H4-H6**: Rarely needed. If you find yourself at H4 depth, the content may be better served by splitting into separate items.

### Heading Quality

Headings serve two audiences: readers scanning the page, and search engine crawlers building a content model. Both benefit from headings that are complete thoughts.

**Label headings** like "Background," "Details," or "Conclusion" waste the heading's semantic value. They tell neither the reader nor the crawler what the section contains.

**Descriptive headings** like "Why Connection Pooling Matters for Serverless Functions" communicate the section's value in isolation. A reader scanning headings can extract the page's core arguments without reading the body. A crawler can build a richer content model.

**Question headings** like "How Does Connection Pooling Work?" are particularly valuable because they match how people phrase search queries. Google frequently extracts the heading + first paragraph as a featured snippet for question-format queries.

### The Skip-Level Problem

Jumping from H2 to H4 (skipping H3) signals structural incoherence to both accessibility tools and search engines. It suggests that the author did not think carefully about content organization, which undermines the expertise signal.

If a section does not need sub-headings, do not add them. An H2 followed by body text is perfectly fine. But if sub-headings are needed, they must be H3s.

## Image Alt Text

Images in Pignal content use standard markdown syntax (`![alt text](url)`). The alt text attribute serves two purposes:

1. **Accessibility.** Screen readers read alt text aloud to visually impaired users.
2. **SEO.** Search engines cannot see images. Alt text is the only way they know what an image depicts.

### Writing Effective Alt Text

**Describe what the image shows, not what it is.** "Screenshot of a terminal" is not useful. "Terminal output showing three failed CORS preflight requests with 403 status codes" is useful -- it tells both screen reader users and search engines what information the image conveys.

**Include relevant keywords naturally.** If the image shows a Cloudflare Workers dashboard, mention "Cloudflare Workers" in the alt text. But do not stuff keywords -- describe the image honestly.

**Keep it concise.** Alt text should typically be one sentence. If you need more than two sentences, the image may be carrying too much informational weight and should be supported by surrounding text.

**Skip decorative images.** If an image is purely decorative and adds no informational value (a divider line, a background pattern), use empty alt text (`![](url)`) so screen readers skip it.

## URL Structure

In Pignal, the URL for a vouched item is `/item/:slug`. The slug is set permanently when you call `vouch_item`. Because changing a slug later breaks existing links and loses accumulated SEO equity, getting it right the first time matters.

### Slug Principles

**Readable by humans.** A good URL can be read aloud and understood: `/item/cloudflare-workers-cors-fix` communicates the topic. `/item/post-47` communicates nothing.

**Keyword-inclusive.** The slug should contain the primary keyword for the page. Search engines use URL words as a relevance signal, and users see the URL in SERPs.

**Short.** 3-6 words is the sweet spot. Shorter URLs are easier to share, easier to remember, and look cleaner in SERPs. Long URLs get truncated and suggest unfocused content.

**No dates.** URLs should be permanent identifiers. A date in the URL (`/item/2024-03-react-hooks`) makes the content look stale after that date passes, even if the content is still accurate.

**No stop words.** Words like "the," "and," "a," "in" add length without adding search value. "React Server Components Data Fetching" becomes `react-server-components-data-fetching`, not `how-to-use-the-react-server-components-for-data-fetching`.

**Lowercase and hyphenated.** Pignal normalizes slugs to lowercase automatically. Hyphens are the standard word separator in URLs (not underscores, not camelCase).

### Slug Permanence

Once a slug is set via `vouch_item`, treat it as permanent. Reasons:
- External sites may link to the URL. Changing the slug breaks those links.
- Search engines have indexed the URL. Changing it causes a temporary ranking drop while the new URL is crawled and indexed.
- Social media shares reference the URL. Shared links break when slugs change.

If a slug must change (rare), the ideal solution is a 301 redirect from the old URL to the new one. Pignal does not currently support redirects natively, so slug changes should be avoided.
