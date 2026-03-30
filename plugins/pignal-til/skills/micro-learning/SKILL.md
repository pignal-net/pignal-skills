---
name: micro-learning
description: |
  Micro-knowledge capture craft for Pignal TIL (Today I Learned) sites.
  Concise entry format, atomic idea capture, daily grouping, and knowledge
  compression techniques. Use this skill whenever the user wants to capture
  a quick learning, write a TIL entry, save a micro-note, log daily
  learnings, or operate a Pignal TIL site. Also use when someone says
  "I just learned", "TIL", "quick note about", or wants to capture
  something brief they discovered.
allowed-tools: [mcp__*]
---

# Micro-Knowledge Capture

TIL (Today I Learned) entries are the smallest useful unit of shared knowledge — one idea, clearly expressed, immediately valuable. The craft is in compression: saying enough to be useful while saying little enough to be quick.

## Working with Your Pignal Site

Before creating entries, discover your site's configuration:

1. Call `get_site_tools` to see available capabilities
2. Call `get_metadata` (via `call_site_tool`) to learn:
   - Content types and their guidance patterns
   - Quality guidelines and character limits
   - Available tags and workspaces

Combine this with the micro-learning principles below.

## The Art of Atomic Ideas

### One Concept Per Entry

Each TIL should capture exactly one idea. This matters because:
- **Recall improves with specificity.** "I learned three things about CSS Grid" is harder to retrieve mentally than three separate TILs, each about one specific Grid behavior.
- **Discoverability improves.** A reader searching for "CSS Grid auto-fill" will find a focused TIL but might miss it buried in a multi-topic entry.
- **Tagging is more precise.** One concept = one clear set of tags.

**Test:** If you need the word "also" or "additionally," you probably have two TILs.

### Concise Writing: Compress Without Losing Meaning

Micro-entries reward brevity, but not at the cost of clarity.

**Good compression:**
- Remove throat-clearing ("I was working on a project when I realized...")
- Lead with the fact, not the story
- Use code blocks instead of describing code in prose
- Let examples do the explaining

**Bad compression:**
- Removing context that makes the entry understandable
- Using jargon without any explanation
- Being so terse that the entry is only useful to you

**Pattern:** `[What I learned] + [Why it matters or when it applies] + [Example if needed]`

### Title Craft for TILs

The keySummary is everything in a TIL — it's often all a reader sees when scanning.

**Effective patterns:**
- "CSS Grid: `auto-fill` vs `auto-fit` — when each applies"
- "`git bisect` can find the commit that introduced a bug in O(log n)"
- "Python's `walrus operator` `:=` works inside list comprehensions"

**What makes these work:** They're specific enough to be useful even without reading the body. A reader scanning titles can learn from the title alone.

**Avoid:** Vague titles like "Interesting CSS thing" or "Something about Git."

## Daily Capture Habits

The TIL format works best as a daily practice — a habit of noticing and recording what you learn.

### When to Capture

- **During work:** When you solve a problem in an unexpected way
- **During reading:** When a blog post, doc, or conversation reveals something new
- **During debugging:** When the root cause surprises you
- **During review:** When someone else's code teaches you a pattern

### Capture Fast, Polish Later

Use `save_item` with `private` visibility for rough captures. The goal during capture is speed — don't let perfectionism slow you down. You can polish and `vouch` later.

### When a TIL Should Become a Blog Post

If you find yourself writing more than ~300 words, or if the idea needs multi-step explanation, context-setting, or multiple examples — it's not a TIL anymore. Promote it to a full blog post or guide.

**Rule of thumb:** If it takes more than 5 minutes to write, it's probably not a TIL.

## Tagging Strategy for Micro-Entries

TIL tags should be broader than blog tags because entries are smaller:

- **Use technology/tool tags:** `css`, `git`, `python`, `docker`
- **Use concept tags:** `performance`, `security`, `debugging`
- **Avoid overly specific tags** that will only have one entry

2-3 tags per TIL is the sweet spot. The goal is to create useful filtered views where a reader can browse "all Git TILs" or "all security TILs."

## Source Attribution

When you learn from someone else's work, attribute it:

- Link to the source in the content
- Mention the author if applicable
- This isn't just courtesy — it builds a network of references that helps readers (and you) trace ideas back to their origins

## Common Mistakes

- **Too long:** If it's more than a few paragraphs, it's not a TIL. Split or promote.
- **Too vague:** "Learned about Docker networking" teaches nothing. What specifically?
- **No example:** For technical TILs, a code snippet is often worth more than the explanation.
- **Duplicate capture:** Before saving, check if you've already captured this. Use `search_items` to verify.

## Workflow

1. **Discover** — `get_metadata` to understand types and limits
2. **Capture** — `save_item` with private visibility, focused keySummary
3. **Tag** — 2-3 broad tags for discoverability
4. **Polish** (optional) — Refine wording, add example
5. **Publish** — `vouch_item` when ready for public consumption
