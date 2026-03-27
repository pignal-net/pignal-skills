# Information Architecture for Documentation

## What Information Architecture Solves

A documentation site with 200 well-written pages and no organizational structure is a haystack. Information architecture (IA) is the practice of organizing that haystack so readers find the needle — not by searching through everything, but by navigating a structure that matches how they think.

Why this is not just "putting things in folders": Folders are a storage mechanism. IA is a cognitive model. The difference is that a folder structure organizes content for the author. IA organizes content for the reader. These are often not the same structure.

## The Three Organizational Models

Documentation content falls into three models, and most sites need all three working together.

### Topic-Based Organization

Group documents by subject area. All authentication documents together, all deployment documents together, all configuration documents together.

**When it works:** Readers who know what subject they need but not what specific task or detail they are looking for. A developer who knows they need "something about authentication" navigates to the authentication section and browses.

**When it fails:** Topics that span multiple subjects. "How to configure authentication for deployment" does not fit cleanly in either "authentication" or "deployment." Cross-references solve this, but only if the IA acknowledges the overlap.

**In Pignal:** Workspaces map naturally to topic-based organization. Each workspace becomes a section: "Getting Started," "Core Concepts," "API Reference," "Operations."

### Task-Based Organization

Group documents by what the reader is trying to accomplish. "Setting up your first project," "Adding users," "Deploying to production."

**When it works:** Readers who arrive with a specific goal. They do not care about the subject taxonomy — they want to get something done. This is the natural model for how-to guides.

**When it fails:** Readers looking for reference information. Someone who wants to check the parameters of an API endpoint does not think in terms of tasks — they think in terms of the API surface.

**In Pignal:** Tags enable task-based cross-cutting views. Tag documents with `setup`, `migration`, `troubleshooting` so readers can filter by activity regardless of which workspace the document lives in.

### Reference-Based Organization

Group documents by the structure of the system itself. API endpoints organized by resource, configuration files organized by section, CLI commands organized alphabetically.

**When it works:** Experienced users who know the system and need to look up specifics. The reference section is a map of the system, not a guide to using it.

**When it fails:** New users who do not yet have a mental model of the system. Reference organization mirrors the system's structure, which the new user has not learned yet.

### Combining the Models

Effective documentation sites use all three models simultaneously:

- **Workspaces for topics** — "Authentication," "Database," "Deployment"
- **Tags for tasks** — `setup`, `migration`, `debugging`, `monitoring`
- **Diataxis types for document purpose** — Tutorials, how-to guides, reference, explanation

This gives readers three ways to find the same document: by subject, by task, or by the kind of information they need.

## Navigation Design

Navigation is how the reader moves through the documentation. It is the IA made visible.

### The Three-Click Rule

A reader should be able to reach any document within three navigation actions from the entry point. This is not a rigid rule — it is a heuristic that prevents documentation from becoming a deep tree where useful content hides five levels down.

**Apply it practically:**
- Level 1: Top-level sections (workspaces or categories)
- Level 2: Documents within a section
- Level 3: Sections within a document

If your IA requires a fourth level, the categories are probably too granular.

### Entry Points Matter

Readers enter documentation from different directions. Some start at the home page. Some land on a specific page from a search engine. Some follow a link from another document.

**Every page must orient the reader.** A reader who lands on "How to configure rate limiting" from a search result needs to know: Where am I? What is this site about? Where do I go next? Breadcrumbs, section headings, and cross-references provide this orientation.

**The home page is a routing decision.** The documentation home page should not try to teach anything. Its job is to help the reader find the right section. A clear list of sections with one-sentence descriptions outperforms a wall of introductory text.

### Sidebar vs. In-Page Navigation

**Sidebar navigation** works for workspace/category browsing. It shows the structure of the documentation at a glance and helps readers understand where they are in the hierarchy.

**In-page navigation** (table of contents) works for long documents. It lets the reader jump to the specific section they need without scrolling through content they already know.

Both serve the scanner. Both should be based on well-crafted headings.

## Taxonomy Design

Taxonomy is the system of categories, tags, and labels that classify documentation. A well-designed taxonomy makes content findable. A poorly designed one makes it harder to find than no taxonomy at all.

### Workspace Taxonomy

**Flat is better than nested.** Three to seven top-level workspaces serve most documentation sites. More than seven overwhelms the reader's ability to scan. Fewer than three suggests the site does not have enough content to need workspaces yet.

**Name for the reader, not the team.** Internal team names ("Platform Team Docs," "Backend Docs") mean nothing to external readers. Name workspaces for the subject: "API Reference," "Deployment," "Architecture."

**Workspace descriptions are navigation aids.** A workspace named "Operations" is ambiguous — does it mean DevOps? Business operations? Day-to-day usage? A one-line description resolves ambiguity without requiring the reader to click through.

### Tag Taxonomy

Tags serve a different purpose than workspaces. Workspaces are categorical (a document lives in one workspace). Tags are descriptive (a document can have many tags). Together they create a matrix of findability.

**Design tags around reader vocabulary.** Use the terms your readers would type into a search box: `authentication`, `caching`, `error-handling`, `performance`. Not the terms your team uses internally.

**Controlled vocabulary vs. freeform.** A controlled tag vocabulary (a fixed list of allowed tags) prevents the proliferation problem where `auth`, `authentication`, `authn`, and `login` all mean the same thing. Check existing tags with `list_items` or `search_items` before creating new ones.

**Tag cardinality guidelines:**
- 1 tag: Under-categorized, will not appear in useful filtered views
- 2-5 tags: Ideal range
- 6+ tags: Over-categorized, tags lose specificity

## Search Optimization

Even with perfect navigation, some readers will search. IA decisions directly affect search quality.

### keySummary as Search Target

The keySummary is the most heavily weighted field in Pignal's search. A keySummary that accurately describes the document's content makes it findable. A vague or clever keySummary makes it invisible.

**Search-optimized keySummary patterns:**
- Use the same words readers would search for
- Include the subject AND the document type: "Authentication API reference" not just "Authentication"
- Avoid jargon in titles unless the jargon IS what readers search for (e.g., "CORS" is jargon but it is what developers type)

### Tags as Search Facets

Tags create filtered views that function as curated search results. A reader who clicks the `deployment` tag gets every document about deployment, regardless of workspace. This is often more useful than a full-text search because it returns categorized results, not ranked matches.

### Content Structure Aids Search

Well-structured documents surface better in search because search engines can extract meaningful snippets from headings, lists, and table rows. A document that is one long paragraph produces poor search snippets. A document with clear headings and concise section openings produces rich ones.

## Scaling Documentation

Small documentation sites can survive with minimal IA. Large ones cannot. Plan for growth even when starting small.

### Signs Your IA Needs Revision

- Readers frequently ask questions answered by existing documentation (discoverability problem)
- Multiple documents cover the same topic with slightly different information (duplication problem)
- New documents have no obvious home (categorization problem)
- The "Miscellaneous" or "Other" workspace is growing (taxonomy failure)

### IA Maintenance

IA is not a one-time design exercise. As content grows, the original structure may no longer fit. Review the IA periodically:

1. **Audit workspace sizes.** A workspace with 50+ documents probably needs to be split. A workspace with 1-2 documents should be merged or rethought.
2. **Audit tag usage.** Tags with only one document are noise. Tags with 20+ documents might need to be split into more specific terms.
3. **Check for orphans.** Documents that have no tags and are not cross-referenced from anywhere are effectively invisible.
4. **Test with new readers.** Ask someone unfamiliar with the site to find specific information. Where they struggle reveals IA weaknesses.
