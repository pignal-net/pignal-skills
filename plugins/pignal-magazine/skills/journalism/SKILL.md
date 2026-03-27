# News and Editorial Craft

News and editorial craft for Pignal Magazine sites. Inverted pyramid structure, headline writing, lede construction, and news value hierarchy. Use when writing news articles, feature stories, editorial content, or operating a Pignal magazine/news site. Also use for any journalism-style writing.

---

## The MCP Workflow

Every editorial operation follows this sequence. Do not skip steps — each one prevents a different class of mistake.

1. **Discover** — `get_site_tools` to learn what the site can do.
2. **Learn the site** — `call_site_tool` with `get_metadata` to get the site's types, workspaces, limits, and quality guidelines. This is not optional. A magazine site may have types for breaking news, features, opinion, analysis, or reviews — each with different structures and editorial standards.
3. **Assess news value** — Before writing, evaluate the story against the five news value criteria (see "News Value Hierarchy" below). If it fails all five, it is not a story.
4. **Plan** — Decide content type, workspace, tags, and structure based on what `get_metadata` returned and the news value assessment.
5. **Draft** — Write the keySummary (headline) and markdown content following the craft principles in this skill.
6. **Create** — `call_site_tool` with `save_item` to save the article.
7. **Validate** — `call_site_tool` with `validate_item` to apply a quality action. For journalism, this step carries extra weight — it represents editorial review, not just quality assurance.
8. **Publish** — `call_site_tool` with `vouch_item` to set visibility to `vouched` with a clean URL slug.

---

## The Inverted Pyramid

The inverted pyramid is the foundational structure of news writing. It places the most important information first and progressively narrows to supporting details.

### Why It Works

The inverted pyramid is not a stylistic convention — it is an engineering decision for how humans consume information under time pressure.

**Readers leave at every paragraph.** Web analytics consistently show that only 20-30% of readers who start an article reach the end. The inverted pyramid ensures that a reader who leaves after paragraph one still got the core story. A reader who leaves after paragraph three got the core story plus context. Every paragraph adds value without requiring the next one.

**Search engines extract the top.** Featured snippets, social previews, and RSS readers pull from the first 1-2 paragraphs. If the essential information lives there, every distribution channel works automatically.

**Editors can cut from the bottom.** In traditional newsrooms, stories were trimmed to fit available space by removing paragraphs from the end. This only works if the end contains the least essential material. The same principle applies to digital — if a story needs to be shortened, the inverted pyramid makes it safe to trim without losing the core.

### The Three Layers

1. **The Lede (paragraph 1-2):** The essential facts — who, what, when, where, why. A reader should be able to stop here and understand the story.
2. **The Body (paragraphs 3-8):** Supporting details, context, quotes, data. Each paragraph adds depth but does not introduce new essential facts.
3. **The Tail (remaining paragraphs):** Background, history, related developments, lesser quotes. Useful for completeness but not required for comprehension.

### When to Break the Inverted Pyramid

Not every piece of journalism belongs in this structure. Feature stories, profiles, and narrative journalism use different approaches — see "Feature vs. News Structure" below. The decision is based on timeliness and reader intent:

- **Breaking news, event coverage, announcements:** Always inverted pyramid. The reader wants facts fast.
- **Investigative pieces, longform features, profiles:** Narrative or chronological structure. The reader wants a story.
- **Analysis and opinion:** Thesis-evidence structure. The reader wants an argument.

---

## Lede Construction

The lede (first paragraph) is the hardest sentence in journalism to write well. It must simultaneously inform, engage, and set expectations — in 35 words or fewer.

### The Summary Lede

The default for hard news. Answers the most important of the five W's (who, what, when, where, why) in a single sentence.

**Why the summary lede works for hard news:** When readers encounter a news article, they are asking one question: "What happened?" The summary lede answers that question immediately. Anything else — context, backstory, quotes — makes the reader wait for the answer they came for.

**Principles:**
- Lead with the most newsworthy element. If "who" is famous, lead with the name. If "what" is dramatic, lead with the event. If "why" is surprising, lead with the cause.
- One main clause, one idea. Compound ledes that pack two stories into one sentence confuse readers.
- Be specific. "A company announced layoffs" is weak. "Stripe is cutting 14% of its workforce, eliminating roughly 1,120 jobs" is a lede.
- Active voice, past tense for events. "The board voted to..." not "A vote was taken by the board to..."

### The Anecdotal Lede

Opens with a specific scene, person, or moment that illustrates the larger story. Used for features, profiles, and narrative journalism.

**Why the anecdotal lede works for features:** Features answer "what is it like?" rather than "what happened?" An anecdotal lede puts the reader inside a specific moment, creating emotional investment before revealing the broader theme. This works because humans process stories through identification — we understand systems by seeing them affect a single person.

**Principles:**
- The anecdote must directly connect to the story's thesis. A colorful opening that requires a "but the real story is..." transition has failed.
- Keep it to 2-3 sentences maximum. The anecdote is the door, not the room.
- Use concrete sensory detail. "Maria Chen was tired of the commute" is telling. "Maria Chen checked her watch at 6:47 AM, already 12 minutes late for the third time this week" is showing.
- The "nut graf" must follow within 3 paragraphs — the paragraph that tells the reader what the story is actually about.

---

## Headline Writing

The headline is the keySummary field in Pignal. It appears in the feed, search results, social shares, and RSS. A great story with a weak headline reaches no one.

### Core Principles

**Clear beats clever.** Wordplay, puns, and double meanings reward readers who already know the story. Headlines must work for readers who know nothing. "City Council Approves $2M Park Renovation" communicates instantly. "Green Light for Green Spaces" requires the reader to click to understand what happened.

Why this matters: In a feed of 20 headlines, readers make stay-or-skip decisions in under 2 seconds. Clarity survives a 2-second scan. Cleverness does not.

**Active voice, strong verbs.** "Company Launches New API" is stronger than "New API Launched by Company." Active voice names the actor and the action, which is how humans naturally process cause and effect.

**Present tense for immediacy.** News headlines use present tense ("Senate Passes Climate Bill") even though the event is past. This convention creates urgency and is expected by news readers. Features and analysis can use past tense when appropriate.

**Specificity signals value.** "Tech Industry Faces Challenges" could describe any day in any decade. "Apple Cuts iPhone Production by 20% Amid Chip Shortage" names who, what, and why — giving the reader enough to decide whether the story matters to them.

**No clickbait, no question headlines.** "You Won't Believe What the Mayor Said" violates reader trust. "Is This the End of Remote Work?" promises an answer the article rarely delivers. State what happened. If the story is interesting, the facts will demonstrate that without tricks.

### Headline Length

Aim for 8-12 words. Shorter headlines lack specificity. Longer headlines get truncated in feeds and search results. The keySummary character limits from `get_metadata` are the hard boundary — check them.

---

## News Value Hierarchy

Not everything is a story. The five criteria below determine whether something is newsworthy. A strong story satisfies at least two; a lead story satisfies three or more.

### The Five Criteria

**Timeliness** — Did it happen recently? News decays. A story about an event yesterday is news; the same event last month is history. For a Pignal magazine site, timeliness also means relevance to the audience's current concerns — a technology trend piece is "timely" even if no single event triggered it.

Why timeliness matters first: Readers choose news sources based on whether they learn things before or after everyone else. A site that consistently publishes timely content builds an audience that checks back frequently.

**Impact** — How many people does it affect, and how significantly? A policy change affecting 10 million users is more newsworthy than one affecting 100, all else being equal. Impact can be direct (this affects you) or indirect (this changes the industry you work in).

**Proximity** — How close is it to the audience? Proximity is not just geographic — it includes professional proximity (affects your industry), identity proximity (affects people like you), and interest proximity (relates to topics you follow). A Pignal magazine about cloud infrastructure should treat a Cloudflare outage as "local news" even though it is geographically distributed.

**Prominence** — Is a well-known person, organization, or project involved? When a Fortune 500 company changes its API pricing, that is more newsworthy than when an unknown startup does the same thing — not because the startup matters less, but because prominence increases the number of people who care.

**Novelty** — Is this unusual, unexpected, or first-of-its-kind? The classic formulation: "Dog bites man" is not news; "Man bites dog" is. Novelty creates interest because it challenges the reader's existing model of how things work.

### Applying the Hierarchy

Before writing, explicitly assess: which criteria does this story satisfy and how strongly? This assessment should influence both the decision to cover the story and its placement:

- **3+ strong criteria:** Lead story. Invest significant effort in reporting and structure.
- **2 criteria:** Standard coverage. Worth writing but not the top piece.
- **1 criterion:** Brief or aggregated coverage. Consider whether a short mention within a roundup is more appropriate than a standalone article.
- **0 criteria:** Not a story. Do not publish it because it fills space — empty space is better than noise.

---

## Attribution and Sourcing

Journalism's credibility rests on sources. Every factual claim must be traceable to an origin.

### Attribution Principles

**Name your sources.** "According to the company's Q3 earnings report" is verifiable. "Sources say" is not. When writing from public information (press releases, earnings calls, official announcements), cite them explicitly.

**Distinguish fact from interpretation.** "Revenue fell 15% year-over-year" is a fact. "The company is struggling" is an interpretation. Present facts directly; attribute interpretations to their source or clearly label them as analysis.

**Quote with purpose.** Quotes should add voice, authority, or perspective that paraphrasing would lose. "We are committed to excellence" adds nothing — every company says this. "We cut the team by a third because the product-market fit wasn't there" is worth quoting because it reveals honesty and specificity that a paraphrase would dilute.

**Link to primary sources.** When referencing a report, announcement, or document, link to it. This is both good journalism (readers can verify) and good SEO (outbound links to authoritative sources signal content quality).

### When Writing Without Primary Sources

AI-assisted journalism on Pignal often synthesizes publicly available information rather than conducting original interviews. This is legitimate — analysis, synthesis, and context-building are valid editorial functions. Be transparent about it:

- Frame the piece as analysis or commentary, not reporting.
- Cite every factual claim to its public source.
- Use phrases like "public filings show" or "according to the announcement" rather than implying original reporting.

---

## Feature vs. News Structure

### News Structure (Inverted Pyramid)

Used for: event coverage, announcements, breaking developments, data releases.

- Lede: The essential facts in one sentence.
- Body: Supporting details in descending order of importance.
- Tail: Background and context.
- Tone: Neutral, factual, third-person.
- Length: 300-800 words typically.

### Feature Structure (Narrative Arc)

Used for: profiles, trend pieces, investigative stories, explainers.

- Opening: Anecdotal lede or scene-setting (2-3 paragraphs).
- Nut graf: The thesis — what this story is about and why it matters (paragraph 3-4).
- Body: Alternating between narrative (scenes, quotes, anecdotes) and exposition (data, context, analysis).
- Kicker: A closing image, quote, or reflection that resonates with the opening — creating a sense of completeness.
- Tone: Can be more personal, descriptive, and atmospheric than hard news.
- Length: 800-2500 words typically.

Why the distinction matters: Readers have different expectations. A reader clicking a news headline expects to learn what happened in 30 seconds. A reader clicking a feature headline is prepared to invest 5-10 minutes. Using the wrong structure for the wrong format frustrates both types.

---

## Objectivity and Fairness

### The Objectivity Standard

Pure objectivity is a myth — every editorial decision (what to cover, what to lead with, who to quote) involves judgment. What journalism demands instead is **procedural fairness**: a consistent methodology that minimizes bias and maximizes accuracy.

**Present multiple perspectives.** If a policy decision has critics and supporters, represent both. You do not need to give them equal space — false equivalence between a scientific consensus and a fringe position is itself a form of bias. But you must acknowledge that disagreement exists.

**Separate news from opinion.** If the site has distinct types for news and opinion (check `get_metadata`), use the correct one. Readers should always know whether they are reading a factual account or an argued position.

**Label analysis clearly.** When you move from reporting facts to interpreting them, signal the shift. Phrases like "this suggests" or "analysts interpret this as" mark the boundary between what happened and what it means.

### When Objectivity Doesn't Apply

Opinion pieces, editorials, and reviews are explicitly subjective. The standard shifts from objectivity to intellectual honesty — state your position clearly, acknowledge counterarguments, and support claims with evidence. The reader should know your stance and understand your reasoning even if they disagree.

---

## The keySummary for Journalism

The keySummary is the headline. In journalism, headline-writing is a distinct craft — see "Headline Writing" above. Key patterns for news:

**News headline patterns:**
- Event: "OpenAI Releases GPT-5 with Native Tool Use"
- Impact: "New EU Regulation Will Require AI Disclosure on All Generated Content"
- Conflict: "Apple and Epic Resume Legal Battle Over App Store Fees"
- Change: "GitHub Drops Free Tier for Copilot, Moves to Usage-Based Pricing"

**Feature headline patterns:**
- Profile: "Inside Cloudflare's Bet on AI at the Edge"
- Trend: "Why Startups Are Abandoning Microservices for Monoliths"
- Explainer: "How Browser Fingerprinting Works and Why It's Getting Harder to Block"

**Anti-patterns for journalism headlines:**
- First-person: "I think..." (save for opinion pieces)
- Questions: "Will AI Replace Developers?" (state what you know, don't ask what you don't)
- Vague attribution: "Experts Say..." (which experts?)

---

## Publishing Workflow for News

Timeliness pressure changes the publishing workflow. For breaking news, speed matters — but accuracy matters more. A correction costs more credibility than a 30-minute delay.

1. **Create with high confidence facts only** — Use `save_item` with the confirmed facts. Do not speculate in the initial article.
2. **Validate** — Use `validate_item` immediately. For news, validation represents editorial sign-off that the facts are verified.
3. **Publish** — Use `vouch_item` with a descriptive, keyword-rich slug. News slugs should include the key subject: `openai-releases-gpt5-native-tool-use`.
4. **Update if needed** — For developing stories, update the existing article rather than creating duplicates. Add an "Updated" note at the top with a timestamp.

For features and analysis, the pace is different — quality over speed. Use the same workflow but invest more time in drafting and self-review before validation.

---

## References

For deeper exploration of specific topics:

- **[Inverted Pyramid](references/inverted-pyramid.md)** — Detailed structure breakdown, worked examples, and guidance on when to break from the pyramid.
- **[Headline Craft](references/headline-craft.md)** — Headline formulas, SEO vs. social headline optimization, and A/B testing principles.
