---
name: terminology
description: |
  Terminology and reference craft for Pignal Glossary sites. Clear
  definition writing, lexicography principles, alphabetical organization,
  and cross-referencing. Use this skill when defining terms, building
  glossaries, creating reference tables, managing terminology, or
  operating a Pignal glossary site. Also triggers when asked to define
  something, explain terminology, or create a reference guide.
allowed-tools: [mcp__*]
---

# Terminology and Definition Craft

A glossary is a promise to readers: "If you encounter a term you don't understand, you can look it up here and get a clear, consistent answer." The craft is in writing definitions that actually deliver on that promise — clear enough for newcomers, precise enough for experts.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` (via `call_site_tool`) to learn:
   - Content types (term categories)
   - Workspaces (topic sections)
   - Quality guidelines and limits

## The Craft of Clear Definitions

### The Genus-Differentia Pattern

The most reliable definition structure: **"A [term] is a [category] that [distinguishing features]."**

- "A **load balancer** is a network device that distributes incoming traffic across multiple servers."
- "A **monorepo** is a version control strategy where multiple projects share a single repository."

**Why this works:** It places the term in a known category (genus) then explains what makes it unique (differentia). The reader anchors understanding on something familiar before learning the new part.

### Avoiding Circular Definitions

Never define X using X, or using terms that require X to understand.

- **Bad:** "A recursive function is a function that uses recursion."
- **Good:** "A recursive function is a function that calls itself, typically with a modified argument, until reaching a base condition that stops the chain."

**Test:** Could someone who doesn't know the term understand your definition? If you used words they'd need to look up, you've just moved the problem.

### Scope Management

Keep definitions focused on what the reader needs to know, not everything you know.

- **Too narrow:** "Docker is a containerization tool for Linux." (Misses macOS, Windows, and what containerization means.)
- **Too broad:** "Docker is a technology company that..." (They're looking up the tool, not the company.)
- **Just right:** "Docker is a platform for building and running applications in isolated containers — lightweight, portable environments that bundle code with its dependencies."

### Examples and Usage Context

Include examples when:
- The term is abstract ("idempotent" benefits from a concrete example)
- The term is commonly misused ("authorization" vs "authentication")
- The term has domain-specific meaning ("sprint" in agile vs athletics)

Keep examples short — one sentence or a code snippet. The definition does the heavy lifting; the example confirms understanding.

## Cross-Referencing

### When to Cross-Reference

- **"See also"** — related terms that expand understanding
- **"Compare with"** — terms that are easily confused
- **"Not to be confused with"** — terms that look similar but differ

### How to Cross-Reference

Reference related terms in the content naturally: "Unlike **authentication** (verifying identity), authorization determines what a verified user is permitted to do."

This helps readers build a mental model of how terms relate, not just what each one means in isolation.

## Alphabetical Organization

### Why Alphabetical Works

Alphabetical ordering is the universal lookup convention for reference material. Readers don't need to understand your category system — they just need to know the first letter of the term.

Pignal's glossary template is designed for this pattern. Use workspaces for topic grouping if needed, but the primary sort should be alphabetical.

### Handling Abbreviations and Acronyms

- Define the acronym at its spelled-out form: "**API** (Application Programming Interface) — a set of..."
- Create entries at both the acronym and the full term if both are commonly searched
- Always spell out on first use, even if the acronym is common in your field

## Term Evolution

Definitions aren't permanent. Technology evolves, and terms shift meaning.

- **Version definitions explicitly** when a term's meaning has changed: "Historically, 'serverless' meant... Today, it typically refers to..."
- **Use validation** (`validate_item`) to mark definitions as current and reviewed
- **Revisit periodically** — use `list_items` to audit for stale definitions

## Quality Checklist

Before publishing a definition:

- [ ] Could a newcomer understand this without looking up other terms?
- [ ] Is the definition specific enough to distinguish this term from similar ones?
- [ ] If technical, is there a concrete example?
- [ ] Are related terms cross-referenced?
- [ ] Is it concise? (Most definitions should be 1-3 sentences + optional example)

## Workflow

1. **Discover** — `get_metadata` to understand types and organization
2. **Draft** — `save_item` with genus-differentia structure, private visibility
3. **Cross-reference** — Link to related terms in content
4. **Review** — Apply quality checklist, check for circular definitions
5. **Publish** — `validate_item` then `vouch_item`
