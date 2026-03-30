---
name: resource-curation
description: |
  Resource curation craft for Pignal Directory sites. Resource evaluation,
  category design, status tracking, and description writing. Use when
  curating resources, organizing directories, or managing link collections.
allowed-tools: [mcp__*]
---

# Resource Curation Craft

A directory is a quality filter. The internet has no shortage of resources — it has a shortage of trustworthy guidance about which resources are worth a person's time. The craft of curation is in evaluation, not collection. Anyone can compile links; the curator's value is in deciding which links belong and which do not.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (resource categories), workspaces (domains/topics), and limits

---

## Evaluation Criteria

Every resource added to a directory should pass through explicit evaluation criteria. Without criteria, a directory grows by accumulation rather than judgment — and accumulation produces noise, not signal.

### Authority

**What it means:** Is the resource produced by someone or something with credible expertise in the subject?

**Why it matters:** A tutorial on cryptography written by a security researcher at a reputable institution is more trustworthy than one written by an anonymous blogger. Authority is not about credentials for their own sake — it is about the probability that the information is correct and the advice is sound.

**How to assess:**
- Who created the resource? What is their background in this domain?
- Is the resource associated with an organization known for quality in this field?
- Do other respected sources in the domain reference or recommend this resource?
- Does the resource demonstrate deep knowledge, or does it read like a surface-level summary?

**The trap to avoid:** Authority is not popularity. A viral blog post can be authoritative if the author has genuine expertise. A well-known website can host mediocre resources. Evaluate the specific resource, not just the platform.

### Recency

**What it means:** Is the resource current, or has the subject evolved since it was published?

**Why it matters for different domains:** In rapidly evolving fields (web frameworks, cloud services, AI/ML), a tutorial from two years ago may reference deprecated APIs, obsolete patterns, or tools that no longer exist. In stable domains (data structures, mathematical foundations, writing craft), a resource from a decade ago may be perfectly current.

**How to assess:**
- When was it last updated? (Check the page, not just the publication date — some resources are maintained.)
- Has the technology/field changed significantly since publication?
- Does the resource reference specific versions? Are those versions still supported?

**The nuance:** Do not reject resources solely for age. A 2015 article on database normalization is likely still accurate. A 2015 article on React best practices is likely outdated. The key question is: has the subject changed, not has time passed.

### Relevance

**What it means:** Does this resource serve the directory's audience?

**Why it matters:** A directory about front-end web development should not include a resource on embedded systems programming, even if that resource is excellent. Relevance is evaluated against the directory's scope, not the resource's quality.

**How to assess:**
- Does the resource's topic fall within the directory's declared scope?
- Is the resource's target audience aligned with the directory's audience? (A beginner directory should not include resources targeted at PhD researchers, even if the topic matches.)
- Does the resource add something that existing directory entries do not? If five entries already cover "intro to Git," a sixth needs to offer a distinct angle or audience to earn inclusion.

---

## Description Writing

The description is the curator's primary contribution. It is not a summary of the resource — it is a recommendation that tells the reader what to expect and why this specific resource was chosen.

### Why Descriptions Are Not Summaries

The resource itself is a click away. The reader does not need you to summarize it — they need you to tell them whether it is worth clicking. A summary says "This tutorial covers React hooks." A description says "The clearest introduction to React hooks I've found — starts with useState and builds to custom hooks through practical examples. Best for developers who already know React class components."

### Description Principles

**State what makes this resource worth including.** The description answers: "Why did the curator choose this over the dozens of alternatives?" The answer might be clarity, depth, a unique approach, exceptional examples, or a specific audience fit.

**Identify the target reader.** "Best for beginners" or "assumes familiarity with SQL" helps the reader self-select. A resource that is excellent for experts may frustrate beginners, and vice versa. The curator's job is to match resources to readers.

**Be honest about limitations.** "Comprehensive but assumes a Mac development environment" or "Excellent core content but the exercises are outdated" helps the reader calibrate expectations. Honest descriptions build trust in the directory.

**Keep descriptions concise.** Two to four sentences is the sweet spot. The description must convey enough to inform a decision without becoming a resource itself. If you need more space, the resource might deserve its own review rather than a directory listing.

### Description Anti-Patterns

**Promotional language.** "An amazing, must-read resource!" This tells the reader nothing. Every entry in the directory is implicitly recommended by its inclusion — the description should explain *why*, not just insist.

**Copy-paste from the resource.** Using the resource's own description or tagline as the directory entry makes the curator redundant. The reader could get that from Google.

**Technical specs only.** "A 47-page PDF covering chapters 1-12 of the specification." This describes the container, not the contents. What will the reader learn?

---

## Category Design

Categories are the directory's information architecture. They determine how readers navigate and whether they find what they need.

### MECE Categories

MECE (Mutually Exclusive, Collectively Exhaustive) is the gold standard for categorization.

**Mutually Exclusive:** Each resource belongs in exactly one category. If a resource could fit in two categories, the categories overlap, and the reader does not know where to look. The fix is either to redefine the categories so they do not overlap, or to choose the primary category and use tags for secondary classification.

**Why overlap is harmful:** A reader looking for "CSS layout resources" who finds some in "CSS" and some in "Layout" and some in "Front-End Fundamentals" does not have a directory — they have a scavenger hunt.

**Collectively Exhaustive:** Every resource in the directory's scope fits into a category. If you find yourself creating a "Miscellaneous" or "Other" category, the categories are not exhaustive. Rethink the structure so every resource has a clear home.

### Category Naming

**Name for the reader's mental model, not your organizational convenience.** "Web Performance" is a topic a reader would search for. "Category 7" is an artifact of your internal process.

**Use nouns, not verbs.** "Authentication" not "Authenticating." Categories are topics, not activities.

**Parallel construction.** If one category is "Databases," the next should be "Networking," not "How to Network." Consistent grammatical structure makes the category list scannable.

### Hierarchy Depth

**Two levels maximum for most directories.** A top-level category ("Back-End Development") with subcategories ("Databases," "APIs," "Authentication") is navigable. Adding a third level ("Databases > SQL > PostgreSQL > Extensions") creates a maze that only the curator can navigate.

**Why flat is better:** Readers do not share the curator's mental model. Deep hierarchies assume the reader knows where things are filed. Shallow hierarchies assume the reader will scan options — which is what they actually do.

---

## Status Tracking

Resources change. URLs break, content becomes outdated, free resources become paid. A directory that does not track resource status decays into a graveyard of dead links and stale recommendations.

### Status Categories

**Active** — The resource is available, current, and still recommended. This is the default state.

**Outdated** — The resource's content is no longer current, but may still have historical or conceptual value. Mark it clearly so readers calibrate their expectations.

**Deprecated** — The resource has been superseded by something better. Link to the replacement. Keep the entry for a transition period so bookmarks and external links do not break.

**Broken** — The URL is dead. Remove or replace the link. A directory with broken links is a directory that has not been maintained, and users will stop trusting it.

### Maintenance Cadence

**Quarterly review minimum.** Every directory entry should be checked at least once per quarter:
- Is the URL still live?
- Is the content still current?
- Is the resource still the best option for its niche? Has something better appeared?
- Is the description still accurate?

**Why maintenance matters more than growth:** A directory with 50 current, verified entries is more valuable than one with 500 entries where 20% are broken and 30% are outdated. Quality requires maintenance, not just initial curation.

---

## Quality Over Quantity

### The Curator's Dilemma

There is always pressure to add more. More entries feel like more value. But directories suffer from a paradox: the more entries they contain, the harder it is for readers to find the best one. A directory with 10 entries on a topic forces the reader to evaluate all 10 and determine which one suits their needs — and the reader came to the directory precisely because they did not want to do that evaluation themselves.

**The principle:** For each topic, include the best 2-3 resources that serve different audiences or angles. Not every resource that is "good enough" deserves a slot. The curator's job is to surface the best, not to catalog the acceptable.

### When to Exclude Good Resources

- **Redundancy.** If an existing entry covers the same topic for the same audience with the same quality, the new resource does not add value.
- **Better alternatives exist.** A good resource does not earn inclusion when an excellent resource on the same topic is already listed.
- **Scope drift.** A resource may be excellent but not relevant to the directory's defined scope. Adding it dilutes the directory's focus.

---

## The keySummary

The keySummary is the resource's display name in the directory. It should identify the resource clearly and communicate what kind of resource it is.

**Good patterns:**
- Title + type: "MDN Web Docs: CSS Grid Layout Guide"
- Tool + purpose: "Lighthouse: Performance Auditing Tool"
- Author/source + topic: "Julia Evans: Networking Zines"

**Anti-patterns:**
- URL as title: "https://developer.mozilla.org/en-US/docs/..."
- Vague: "Useful CSS Resource"
- Redundant: "A Resource About CSS Grid for Learning CSS Grid"

---

## Workflow

1. **Discover** — `get_site_tools` then `get_metadata` for resource types and workspaces (categories)
2. **Evaluate** — Apply authority, recency, and relevance criteria before adding
3. **Describe** — Write a curator's recommendation, not a summary
4. **Add** — `save_item` with resource name as keySummary, description + link as content
5. **Validate** — `validate_item` to mark as verified (link checked, description reviewed)
6. **Publish** — `vouch_item` with a clean slug
