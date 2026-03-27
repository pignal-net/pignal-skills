# Headline Craft for News and Editorial

## Why Headlines Deserve Dedicated Attention

A headline is not a label — it is a contract. It tells the reader what they will receive in exchange for their time. A headline that overpromises creates disappointment; one that underpromises gets ignored. The craft is in writing headlines that accurately represent the story's value while maximizing the chance a reader will engage.

In the context of Pignal, the keySummary field serves as the headline. It appears in the site feed, search engine results, RSS readers, and social shares. Every distribution channel renders it differently, but every channel relies on it as the primary decision point for reader engagement.

---

## Headline Formulas That Work

These are not rigid templates — they are patterns that consistently perform well because they align with how readers scan and evaluate.

### The Declarative Statement

States a fact directly. The default for hard news.

- "GitHub Drops Free Tier for Copilot, Moves to Usage-Based Pricing"
- "Cloudflare Workers Now Support Python with Full Standard Library Access"
- "React 20 Introduces Server-First Architecture, Drops Client Components"

**Why it works:** Declarative headlines respect the reader's time. The reader learns the core fact without clicking. Paradoxically, this increases clicks — because the reader trusts that the article contains more value than the headline alone.

### The Consequence Frame

Focuses on impact rather than event. Effective when the "so what" is more interesting than the "what."

- "New Data Privacy Law Will Force Every SaaS Product to Redesign Its Onboarding"
- "Rising Cloud Costs Push Startups Back to Bare Metal Servers"
- "AI Code Review Tools Are Eliminating the Junior Developer Bottleneck"

**Why it works:** Consequence headlines answer the reader's real question — "how does this affect me?" — before they even click. They self-select the right audience: people who care about the consequence will read; those who don't will skip, which is the right outcome.

### The Contrast/Tension Frame

Juxtaposes two elements to create tension that the article resolves.

- "Apple Invests $10B in AI While Cutting 5% of Hardware Staff"
- "Open Source LLMs Match GPT-4 Quality at 1/10th the Cost"
- "Developers Love Rust's Safety — and Hate Its Compile Times"

**Why it works:** Tension creates an information gap that the reader wants to close. Unlike clickbait questions, contrast headlines provide real information (both sides of the tension) while still creating motivation to read the resolution.

### The Scope/Scale Frame

Uses specific numbers or scope to signal significance.

- "AWS Outage Affects 300,000 Websites Across 47 Countries for Six Hours"
- "Survey: 73% of Developers Now Use AI Coding Tools Daily, Up from 23% Last Year"
- "One Developer's Side Project Now Powers 12 Million Monthly Active Users"

**Why it works:** Specificity is the strongest credibility signal in a headline. "Major outage" is vague; "300,000 websites across 47 countries" is concrete and verifiable. Numbers create anchoring — the reader immediately understands the scale and can judge whether it warrants their attention.

### The Explanatory Frame

Signals that the article will help the reader understand something complex.

- "How Browser Fingerprinting Works and Why Ad Blockers Can't Stop It"
- "Inside the Architecture That Lets Figma Handle 10 Million Concurrent Users"
- "What Actually Happens When Kubernetes Reschedules a Pod"

**Why it works:** Explanatory headlines promise knowledge transfer. They appeal to readers who want to understand systems, not just events. The implicit contract is: "You will understand this better after reading."

---

## SEO Headlines vs. Social Headlines

The same story often benefits from different headlines depending on the distribution channel. On Pignal, the keySummary serves both — so the goal is to write headlines that perform across channels.

### What SEO Rewards

- **Primary keyword in the first 4-5 words.** Search results truncate at ~60 characters. Front-load the terms people search for. "PostgreSQL Performance Tuning: 5 Queries That Cut Our Latency in Half" puts the keyword first.
- **Specificity.** Search engines reward headlines that closely match query intent. "How to Fix CORS Errors in Cloudflare Workers" will rank for that exact query better than "Debugging Common Worker Issues."
- **Informational structure.** "How to," "What is," "Why," and "Guide to" signal to search engines that the content answers a question — increasing the chance of a featured snippet.

### What Social Rewards

- **Emotional resonance.** Social sharing is driven by "I want my followers to see this" motivations. Headlines that provoke surprise, validation, or usefulness get shared.
- **Conversation-starting specificity.** "73% of Developers Now Use AI Tools Daily" gets quoted and debated. "AI Tools Are Popular" does not.
- **Completeness.** The best social headlines are self-contained insights — shareable even without clicking the link. This seems counterintuitive, but sharing an insight makes the sharer look knowledgeable, which motivates the share.

### Bridging the Gap

For Pignal, where the keySummary is both the SEO title and the social headline, optimize in this order:

1. **Clarity first.** Both channels reward clarity. Ambiguous headlines fail everywhere.
2. **Keywords second.** Include the primary search term naturally. If it feels forced, the headline needs rewriting, not keyword insertion.
3. **Specificity third.** Numbers, names, and concrete details perform on both channels.
4. **Emotional resonance last.** If you can add an element of surprise or consequence without sacrificing clarity, do it. If not, clarity wins.

---

## Headline Anti-Patterns

### The Question Headline

"Will AI Replace Developers?" — This abdicates the journalist's job. The reader came for answers, not questions. If you know the answer, state it: "AI Is Reshaping Developer Roles, Not Replacing Them." If you don't know the answer, the article probably isn't ready to publish.

**Exception:** Rhetorical questions that signal a counterintuitive answer can work in opinion pieces: "Is TypeScript Making Us Worse at JavaScript?" But this is rare and risky.

### The Vague Teaser

"What We Learned About Performance" — Learned what? Whose performance? This headline contains zero information. Every word in a headline must do work.

### The Clickbait Pattern

"You Won't Believe What Happened When We Deployed to Production" — This trades short-term curiosity for long-term trust. Readers who click feel manipulated; readers who don't learn to ignore the source. News organizations that rely on clickbait see declining return-visit rates because the promise-to-delivery ratio erodes reader trust.

### The Passive Voice Headline

"New Policy Announced by Board" — Passive voice hides the actor. "Board Announces New Policy" is shorter, clearer, and gives the reader the subject-verb-object structure their brain processes fastest.

### The Compound Headline

"Company Launches New Product and Announces Partnership and Reveals Revenue Numbers" — Three stories crammed into one headline. Each deserves its own article. If they must be covered together, lead with the most newsworthy: "Company Revenue Hits $1B; New Product and Partnership Announced."

---

## Headline Length Guidelines

- **8-12 words** is the sweet spot for news headlines. Enough for specificity, short enough for scanning.
- **Under 60 characters** for the first meaningful phrase, to survive search result truncation.
- **Under 100 characters total** for full social media display without truncation on most platforms.

Always check the keySummary character limits from `get_metadata` — these are the hard constraints that override general guidelines.

---

## Testing Your Headline

Before committing to a headline, run these mental tests:

1. **The scan test.** If this headline appeared in a feed of 20 others, would you stop scrolling? If not, it needs more specificity or consequence.
2. **The stranger test.** Would someone with no context about this topic understand what the article is about? If not, you've assumed too much shared knowledge.
3. **The accuracy test.** Does the headline precisely represent what the article delivers? If the article is narrower or broader than the headline suggests, one of them needs to change.
4. **The uniqueness test.** Could this headline appear on five different articles? If so, it's too generic. Add the detail that makes this story distinct.
