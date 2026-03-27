---
name: list-curation
description: |
  Awesome-list curation craft for Pignal Curated List sites. Inclusion
  criteria, quality standards, and community curation. Use when building
  curated lists, awesome-lists, or link collections.
---

# Awesome-List Curation Craft

An awesome list is a promise: every entry on this list is worth your time. The word "awesome" is not decoration — it is a quality bar. When an entry is not awesome, it degrades the list and the trust readers place in it. The craft is in maintaining that bar through clear criteria, concise descriptions, and disciplined curation.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (entry categories), workspaces (sections), and limits

---

## The Awesome-List Convention

The awesome-list format originated with [sindresorhus/awesome](https://github.com/sindresorhus/awesome) on GitHub and has become a recognized pattern for curated resource collections across the software community. While Pignal curated lists do not need to follow GitHub conventions literally, the principles that made the format successful are worth understanding and applying.

### Why the Format Works

**Scanability.** A reader should be able to scan the entire list and identify resources of interest in under 30 seconds. This requires consistent formatting, concise descriptions, and logical grouping. The moment a list requires deep reading to find what you need, it has failed as a curation tool.

**Implicit quality bar.** The format's reputation means readers arrive expecting high quality. Every entry that fails to meet that expectation costs more credibility than the list gains from the entries that do meet it. One mediocre entry in a list of 50 excellent ones damages the list disproportionately.

**Community trust.** Awesome lists succeed because readers trust that someone with expertise has filtered the noise. This trust is earned through consistent quality and lost through lazy inclusion.

---

## Inclusion Criteria

### Defining "Awesome"

Every curated list needs explicit inclusion criteria. Without them, the list grows by the curator's mood rather than by consistent standards — and mood-based curation produces inconsistent quality that readers can sense even if they cannot articulate why.

**Functional criteria:**
- Does the resource work reliably? (For tools/libraries: actively maintained, documented, tested.)
- Does the resource serve its stated purpose well?
- Would you confidently recommend this to a colleague?

**Comparative criteria:**
- Is this the best (or among the best 2-3) resources for its specific niche?
- Does it offer something that existing entries do not?
- If you had to choose between this and an existing entry, would this win?

**Negative criteria (reasons to exclude):**
- Unmaintained projects (no commits in 12+ months for active software domains).
- Resources that require significant caveats to recommend. If the description needs to say "great but the documentation is terrible," it is not awesome.
- Self-promotion without demonstrated quality. A resource created by the curator is not automatically disqualified, but it must meet the same bar as everything else.

### The "Would I Tweet This?" Test

A practical heuristic: if you would not stake your professional reputation on recommending this resource publicly, it does not belong on the list. This test captures the essence of curation — the curator is putting their name behind every entry.

---

## Concise, Factual Descriptions

### Why Brevity Is Not Optional

Each entry gets one line. One sentence. The description must communicate what the resource is and why it is worth the reader's attention — in roughly 10-20 words. This is not a limitation; it is the format's strength. Short descriptions force the curator to identify the single most important thing about each resource.

### Writing One-Line Descriptions

**State what it does, not how it works.** "Fast, zero-config image optimization for web projects" tells the reader whether they need it. "Uses Rust-based codecs with WASM fallbacks and smart caching" tells them how it works — which only matters after they have decided it is relevant.

**Factual, not promotional.** "Lightweight state management library for React" is factual. "The best state management library you'll ever use" is promotional. Promotion in a curated list undermines trust because the reader cannot tell if the entry was included on merit or enthusiasm.

**Differentiate from similar entries.** If the list has three image optimization tools, each description must make clear what distinguishes this one: "Focuses on build-time optimization for static sites" vs. "Runtime optimization with lazy loading and format negotiation" vs. "CLI tool for batch processing local image directories."

### Description Anti-Patterns

| Pattern | Problem | Fix |
|---------|---------|-----|
| "A library for X" | Too generic, no differentiation | Specify the distinguishing characteristic |
| "The best X" | Unsubstantiated superlative | State what makes it good, not that it is good |
| "X, Y, Z, and more" | Vague feature list | Pick the one or two features that matter most |
| No description at all | Reader has no basis to evaluate | Always include a description, even if brief |
| Multi-sentence description | Breaks scanability | Compress to one sentence |

---

## Category Structure

### Two-Level Maximum

Awesome lists use a flat or two-level category structure. Top-level categories group entries by domain. Subcategories provide finer granularity within a domain. A third level is almost never needed and creates navigation overhead that undermines the format's scanability.

**Why deep nesting fails:** The reader is scanning, not navigating a file system. Each level of nesting requires a cognitive decision ("should I expand this?"), and too many decisions cause the reader to give up and use Ctrl+F instead — at which point the category structure has provided no value at all.

### Category Design Principles

**Group by user need, not by technology.** "Testing" (user need: test my code) is better than "Jest, Vitest, Mocha, Cypress" (technology grouping). The reader knows they need to test their code; they may not yet know which tool to use.

**Alphabetical order within categories.** This is a strong convention in awesome lists and serves an important purpose: it prevents the curator's ranking from influencing the reader's choice. If the first entry in a category gets disproportionate attention (which it does — position bias is well-documented), randomizing via alphabetical order is fairer to all entries.

**Why alphabetical beats "best first":** The curator's ranking is one opinion. Different readers have different needs, different contexts, and different preferences. Alphabetical order says "here are the options; you decide." Ranked order says "I've decided for you." The awesome-list format favors the former.

### When Categories Need Splitting

A category with more than 15-20 entries is probably too broad. Scan the entries and look for natural sub-groups. "Databases" might split into "Relational," "Document," and "Key-Value." "Testing" might split into "Unit Testing," "Integration Testing," and "End-to-End Testing."

### When Categories Need Merging

A category with fewer than 3 entries may be too narrow. Consider merging it with a related category or making it a subsection. A category with one entry is not a category — it is an entry that needs a home.

---

## Link Quality

### URL Standards

**Link to the canonical source.** For open-source projects, link to the repository (GitHub, GitLab). For articles, link to the original publication. For tools, link to the official website or documentation. Do not link to mirrors, aggregators, or secondary sources.

**Why canonical links matter:** Canonical links are the most stable and provide the most complete information. A GitHub repository includes the README, issues, stars, activity, and license — all signals the reader can use to evaluate quality. A third-party review of that repository includes one person's opinion.

**HTTPS only.** In the current era, a resource served over HTTP is a resource that is not being maintained. This is a proxy signal for quality.

### Link Maintenance

**Check links quarterly.** Broken links are the most visible form of list decay. A single broken link makes the reader wonder how many others are broken.

**When a link breaks:**
- If the resource moved, update the URL.
- If the resource disappeared, evaluate whether an alternative belongs in the list.
- If no alternative exists, remove the entry. An entry pointing nowhere is worse than no entry.

---

## Contribution Guidelines

If the curated list accepts community contributions (recommended for long-term sustainability), clear contribution guidelines prevent quality degradation.

### Why Guidelines Exist

Without guidelines, contributors add their favorite tools regardless of quality. The list grows but the quality bar drops. Guidelines establish the bar and give the curator a shared reference point for accepting or declining contributions.

### Essential Guidelines

**State the inclusion criteria.** "Resources must be actively maintained, well-documented, and clearly superior or differentiated from existing entries in the same category."

**Require a description.** Entries without descriptions are entries without curation. Require contributors to state what makes the resource awesome.

**One entry per submission.** This allows each entry to be evaluated independently. A submission with five entries forces the curator into all-or-nothing decisions.

**The curator retains final say.** Community contributions are suggestions, not mandates. The curator's judgment is what makes the list trustworthy.

---

## The keySummary

The keySummary is the entry's display name. It should be the resource's official name, optionally with a brief qualifier.

**Good patterns:**
- Official name: "Zod"
- Name + qualifier: "Elysia - Bun web framework"
- Name + context: "Tokio - Async runtime for Rust"

**Anti-patterns:**
- Description as name: "A really good validation library"
- URL as name: "github.com/colinhacks/zod"
- Generic: "Validation Tool"

---

## Workflow

1. **Discover** — `get_site_tools` then `get_metadata` for entry types and workspaces (categories)
2. **Evaluate** — Apply inclusion criteria; reject what is merely "good"
3. **Describe** — One factual sentence on what makes it awesome
4. **Add** — `save_item` with resource name as keySummary, description + link as content
5. **Validate** — `validate_item` to mark as quality-checked
6. **Publish** — `vouch_item` with a clean slug; maintain alphabetical order within categories
