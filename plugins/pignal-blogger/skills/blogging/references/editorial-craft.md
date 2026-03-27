# Editorial Craft for Blog Content

## Content Planning and Topic Ideation

Great blogs don't happen by accident — they come from intentional content planning. The best approach starts with your audience's questions, not your expertise.

### Finding Topics That Resonate

**Mine your conversations.** Every time an AI session produces a genuine insight — something you didn't know before, a problem you solved in an unexpected way, a decision you made with interesting tradeoffs — that's a potential post. The best blog content comes from real experience, not manufactured topics.

**Follow the "Would I bookmark this?" test.** Before committing to a topic, ask: if someone else wrote this, would I save it for later? If no, the topic probably isn't specific or useful enough.

**Go narrow, not broad.** "Getting Started with React" is too broad — thousands of posts cover it. "How I Migrated a 50K-Line Codebase from Class Components to Hooks Without Breaking Production" is specific, experience-based, and genuinely useful.

### Topic Ideation Frameworks

- **Problem → Solution → Insight**: What broke? How did you fix it? What did you learn that others wouldn't expect?
- **Decision Journal**: What did you decide? What were the alternatives? Why did you choose this path?
- **Pattern Recognition**: What have you noticed across multiple projects/situations that others might not see?

## The Draft → Review → Publish Cycle

### Drafting

Write the first draft without editing. Get the ideas out. The goal is completeness, not polish. Use the MCP `save_item` with `private` visibility — this keeps your draft invisible while you work on it.

### Self-Review Checklist

Before publishing, review against these criteria:

1. **Does it pass the "So what?" test?** — If a reader finishes and thinks "so what?", the core value isn't clear enough
2. **Is the opening specific?** — Vague openings ("In today's world...") lose readers instantly
3. **Does every paragraph earn its place?** — Cut anything that doesn't directly support the main point
4. **Are code examples runnable?** — If you include code, it should work. Broken examples destroy trust
5. **Is the conclusion actionable?** — End with what the reader should DO, not just what they should think

### Publishing Strategy

- Start with `private` visibility during drafting
- Move to `unlisted` for sharing with reviewers via token
- `vouch` to make public only when you're confident in the quality
- Use `validate_item` to mark as human-reviewed — this adds E-E-A-T signals

## Voice and Tone Consistency

Your blog's voice is its personality. Consistency matters because it builds trust — readers return when they know what to expect.

**First-person is powerful.** "I discovered..." is more engaging than "It was discovered..." because it signals real experience. Pignal's blog template is designed for first-person knowledge capture.

**Be opinionated but fair.** State your position clearly, but acknowledge alternatives. "I prefer X because Y, though Z is a valid choice if you need W" is stronger than "X is the best."

**Match depth to type.** An Insight can be brief and punchy. A Guide should be thorough and structured. A Problem Solution needs enough context for someone to apply it. Let the content type (discovered via `get_metadata`) guide your depth.

## Audience Awareness

**Write for one person.** Imagine a specific reader — their skill level, their context, what they need. Writing for "everyone" means writing for no one.

**Signal the skill level early.** If your post assumes knowledge of Kubernetes, say so in the first paragraph. Readers who don't have that context will appreciate the honesty; those who do will feel confident the content won't waste their time.

**Anticipate questions.** After each major point, ask yourself: "What would the reader ask here?" Address those questions inline or in a separate section.
