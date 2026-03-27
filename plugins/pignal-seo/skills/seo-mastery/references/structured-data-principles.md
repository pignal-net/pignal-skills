# Structured Data Principles

## What Structured Data Is

Structured data is machine-readable markup that tells search engines what a page is about, not just what words it contains. HTML tells a browser how to display content. Structured data tells a search engine what the content means.

A page about a blog post is just text to a search engine. Structured data says: this is a `BlogPosting`, written by this `Person`, published on this date, about this topic. The search engine can then display the result with rich formatting -- author photo, publish date, star ratings, FAQ dropdowns -- that plain results do not get.

## JSON-LD Format

JSON-LD (JavaScript Object Notation for Linked Data) is the recommended format for structured data. It lives in a `<script type="application/ld+json">` tag in the page head, completely separate from the visible HTML. This separation is the key advantage over older formats (Microdata, RDFa) that require mixing data attributes into HTML elements.

### Basic Structure

Every JSON-LD block follows this pattern:

```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Fixed CORS Errors by Moving Headers to the Worker",
  "description": "CORS errors in Cloudflare Workers happen when...",
  "author": {
    "@type": "Person",
    "name": "Author Name"
  },
  "datePublished": "2024-03-15T10:00:00Z"
}
```

- `@context` tells processors which vocabulary to use (always `https://schema.org` for web content)
- `@type` identifies the entity type
- Properties are key-value pairs defined by the schema.org vocabulary

### Nesting

JSON-LD supports nesting to express relationships. An `author` is not just a string name -- it is a `Person` entity with its own properties:

```json
{
  "@type": "Person",
  "name": "Author Name",
  "url": "https://github.com/username",
  "sameAs": "https://github.com/username"
}
```

The `sameAs` property is particularly valuable because it links the author entity to verified profiles on other platforms, helping search engines build an author knowledge graph.

## Schema.org Vocabulary

Schema.org is the shared vocabulary used by Google, Bing, Yahoo, and Yandex. It defines hundreds of types and thousands of properties. The most relevant types for content sites:

### Content Types

| Type | Use Case | Key Properties |
|------|----------|---------------|
| `BlogPosting` | Blog posts, articles, insights | `headline`, `description`, `author`, `datePublished`, `dateModified`, `articleSection`, `keywords` |
| `Article` | General articles, news | Same as BlogPosting, plus `publisher` |
| `HowTo` | Tutorials, step-by-step guides | `name`, `step` (array of `HowToStep`) |
| `Product` | Products, items for sale | `name`, `description`, `offers`, `brand` |
| `Recipe` | Cooking recipes | `name`, `recipeIngredient`, `recipeInstructions`, `cookTime` |
| `FAQPage` | FAQ pages | `mainEntity` (array of `Question` with `acceptedAnswer`) |
| `Review` | Reviews, evaluations | `itemReviewed`, `reviewRating`, `author` |

### Entity Types

| Type | Use Case | Key Properties |
|------|----------|---------------|
| `Person` | Authors, creators | `name`, `url`, `sameAs`, `jobTitle` |
| `Organization` | Publishers, companies | `name`, `url`, `logo` |
| `WebPage` | Any web page | `@id` (canonical URL), `name`, `description` |
| `WebSite` | Entire website | `name`, `url`, `description`, `author` |
| `Blog` | Blog as a whole | `name`, `url`, `description`, `author` |

## Required vs Recommended Properties

Google distinguishes between required and recommended properties for each structured data type. Required properties must be present for the structured data to be valid. Recommended properties improve the richness of the result.

### For BlogPosting (most common on Pignal)

**Required:**
- `headline` -- the title
- `author` -- who wrote it (at minimum, `author.name`)
- `datePublished` -- when it was published

**Recommended:**
- `dateModified` -- when last updated (freshness signal)
- `description` -- summary of the content
- `image` -- a representative image (enables larger SERP display)
- `publisher` -- the publishing entity
- `mainEntityOfPage` -- canonical URL reference
- `articleSection` -- the content category
- `keywords` -- topic tags

### What Pignal Provides Automatically

Pignal generates all required and most recommended properties from the item data:

| Property | Source |
|----------|--------|
| `headline` | `keySummary` field |
| `author.name` | Settings: `owner_name` > `source_title` > domain hostname |
| `author.sameAs` | Settings: `source_social_github` |
| `datePublished` | `vouchedAt` or `createdAt` |
| `dateModified` | `updatedAt` |
| `description` | First 160 chars of stripped content |
| `publisher` | Site name and URL |
| `mainEntityOfPage` | Canonical item URL |
| `articleSection` | Item type name |
| `keywords` | Item tags (comma-separated) |

The `image` property is the main gap -- Pignal does not currently extract images from markdown content for structured data. If your content includes images and you want image-rich SERP results, this is a known limitation.

## Testing Structured Data

### Google Rich Results Test

The primary testing tool: https://search.google.com/test/rich-results

Paste a URL or a code snippet to validate structured data. The tool reports:
- **Errors** -- required properties that are missing or malformed. These must be fixed for the structured data to be recognized.
- **Warnings** -- recommended properties that are missing. Not blocking, but fixing them improves rich result eligibility.

### Schema Markup Validator

For broader validation beyond Google's specific requirements: https://validator.schema.org/

This tool validates against the full schema.org specification, catching issues that Google's tool might not flag but other consumers (Bing, AI systems) might care about.

### What to Check

After vouching a new item, verify:

1. The JSON-LD is present in the page source (view source, search for `application/ld+json`)
2. The `headline` matches the keySummary exactly
3. The `author.name` resolves to a real name (not "My Pignal" or the domain hostname -- configure `owner_name` in settings)
4. `datePublished` is present and correct
5. `keywords` includes the tags you set
6. `articleSection` matches the content type name

## Rich Results

Structured data enables rich results -- enhanced SERP displays that include additional information beyond the standard title, URL, and description.

### Types of Rich Results

**Article rich results.** Show author, date, and sometimes a thumbnail image alongside the standard SERP entry. Triggered by valid `BlogPosting` or `Article` structured data.

**FAQ rich results.** Show expandable question-answer pairs directly in the SERP. Triggered by `FAQPage` structured data with `Question` and `acceptedAnswer` pairs.

**How-to rich results.** Show step-by-step instructions with optional images. Triggered by `HowTo` structured data with `HowToStep` items.

**Product rich results.** Show price, availability, and review ratings. Triggered by `Product` structured data with `offers` and optionally `aggregateRating`.

**Review rich results.** Show star ratings in the SERP. Triggered by `Review` structured data with `reviewRating`.

### Rich Result Eligibility

Having valid structured data does not guarantee a rich result. Google decides whether to show rich results based on:

1. **Content quality.** Pages with thin or low-quality content may not receive rich results even with valid structured data.
2. **Policy compliance.** Structured data must accurately represent the page content. Misleading structured data (claiming a rating that does not exist on the page) leads to manual actions.
3. **Algorithmic factors.** Google may suppress rich results for specific queries, user contexts, or page types.

The content must be genuinely valuable. Structured data amplifies quality content -- it does not substitute for it.

## Template-Specific Schema Types

Pignal's template system uses `seoHints` to customize the JSON-LD `@type` per template. For example:

- Blog template: `BlogPosting` for items, `Blog` for the source page
- Shop template: `Product` for items, `Store` for the source page
- Recipe template: `Recipe` for items, `WebSite` for the source page

When `@type` is `Product`, the JSON-LD uses `name` instead of `headline` (per schema.org specification -- Products have names, not headlines). This happens automatically based on the template configuration.

Understanding which schema type your template uses helps you write content that maps well to the structured data properties. A `Product` benefits from clear pricing information in the content. A `BlogPosting` benefits from a strong author attribution chain.

## Multiple Structured Data Blocks

A single page can contain multiple JSON-LD blocks. Pignal generates one per page (source page or item page), but the pattern supports extension. For example, a page could have both a `BlogPosting` block and a `BreadcrumbList` block.

Each block is a self-contained JSON-LD object in its own `<script>` tag. Search engines process all blocks on the page and combine them into a unified understanding of the page's entities.
