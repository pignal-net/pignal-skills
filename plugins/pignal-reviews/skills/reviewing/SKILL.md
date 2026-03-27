# Critical Assessment Craft

Balanced evaluation, rating methodology, comparison frameworks, and evidence-based claims for Pignal Review sites. Use when writing product reviews, ratings, comparisons, or any evaluative content.

---

## The MCP Workflow

Every review operation follows this sequence. Do not skip steps — each one prevents a different class of mistake.

1. **Discover** — `get_site_tools` to learn what the site can do.
2. **Learn the site** — `call_site_tool` with `get_metadata` to get the site's types, workspaces, limits, and quality guidelines. This is not optional. A review site may have types for full reviews, quick takes, comparisons, and buyer's guides — each with different depth expectations and structures.
3. **Plan** — Decide content type, workspace, tags, rating criteria, and structure based on what `get_metadata` returned.
4. **Draft** — Write the keySummary, markdown content, and rating following the craft principles in this skill.
5. **Create** — `call_site_tool` with `save_item` to save the review.
6. **Validate** — `call_site_tool` with `validate_item` to apply a quality action. For reviews, validation represents confirmation that claims are evidence-based and the assessment is balanced.
7. **Publish** — `call_site_tool` with `vouch_item` to set visibility to `vouched` with a clean URL slug.

---

## Balanced Evaluation

The defining principle of credible reviewing is balance. Every product, service, or experience has strengths and weaknesses. A review that presents only positives is marketing; one that presents only negatives is a hit piece. Neither serves the reader.

### Why Balance Matters

**Trust is earned through acknowledged weakness.** When a reviewer says "this product excels at X but falls short on Y," the reader trusts the praise because the reviewer has demonstrated willingness to criticize. Unqualified praise ("this is amazing in every way") triggers skepticism because readers know perfection doesn't exist.

**Readers need decision-support, not cheerleading.** The reader's question is "should I choose this?" — not "is this good?" Those are different questions. Answering the first requires discussing tradeoffs, alternatives, and fit. Answering the second only requires enthusiasm.

**Balance protects against obsolescence.** Products change. A balanced review that says "excellent camera but sluggish software — pending updates could change this" remains useful regardless of whether the update ships. A review that says "best phone ever" becomes misleading the moment a better one arrives.

### The Pros-and-Cons Discipline

Every review must explicitly state both strengths and weaknesses. This is non-negotiable. Even products you love have weaknesses; even products you dislike have strengths.

**Structural requirement:** Include a dedicated pros/cons section or integrate pros and cons throughout the review with clear labeling. The reader should never have to guess which parts are praise and which are criticism.

**Proportionality:** The ratio of pros to cons should reflect reality, not a forced 50/50 split. An excellent product might genuinely have 5 pros and 2 cons. The point is not equal weight — it is honest weight.

---

## Rating Criteria and Score Calibration

### Defining Criteria

Before writing a review, define the criteria you will evaluate. Good criteria meet three tests:

1. **Observable.** The criterion can be evaluated through direct experience or measurable data. "Build quality" is observable. "Soul" is not.
2. **Relevant.** The criterion matters to the target reader's decision. "Water resistance" matters for a phone review. "Box design" does not — unless the review covers the unboxing experience specifically.
3. **Comparable.** The criterion can be evaluated across different products in the same category. This enables readers to compare reviews and enables the reviewer to build a consistent body of work.

**Common criteria by domain:**
- **Hardware:** Design, build quality, performance, battery life, display, value
- **Software:** Ease of use, feature set, performance, reliability, documentation, pricing
- **Services:** Quality, speed, support, pricing, flexibility, reliability
- **Content/Media:** Production quality, depth, accuracy, entertainment value, accessibility

### Score Calibration

If the site uses numerical ratings, calibrate them consistently. The most common failure in review scoring is grade inflation — where 7/10 means "mediocre" and anything below 6 means "broken."

**Why calibration matters:** If every review scores 8-10, the rating system provides no signal. Readers cannot distinguish "great" from "good" from "acceptable." A calibrated scale uses its full range and communicates real differences.

**Recommended calibration (10-point scale):**

| Score | Meaning | Frequency |
|---|---|---|
| 9-10 | Exceptional — best in class, minimal flaws | Rare (~5% of reviews) |
| 7-8 | Good — recommended with noted reservations | Common (~40%) |
| 5-6 | Average — serves its purpose, notable weaknesses | Common (~35%) |
| 3-4 | Below average — significant issues, alternatives preferred | Occasional (~15%) |
| 1-2 | Poor — fundamental failures, not recommended | Rare (~5%) |

**Category-specific scoring:** A product's score should reflect how well it serves its target audience, not how it compares to products in a different price bracket. A $200 phone that does everything well for its price deserves a higher score than an $1,100 phone that disappoints relative to expectations. Always state the context within which the score applies.

---

## Review Structure

### The Overview-Features-Performance-Verdict Arc

This four-part structure works because it mirrors the reader's decision process: understand what it is, learn what it does, see how well it does it, and get a recommendation.

**Overview (1-2 paragraphs):**
- What is the product and who is it for?
- Where does it sit in the market? (Price tier, competitor landscape)
- What is the single most important thing the reader should know?

Why the overview matters: Many readers will only read this section. If the product is clearly wrong for them (wrong price, wrong category, wrong use case), the overview should make that clear immediately — saving their time and earning trust.

**Features and Design (3-5 paragraphs):**
- Walk through the key features, explaining what each does and why it matters.
- Cover design decisions — not just aesthetics, but ergonomics, usability, and material choices.
- Compare to the previous version or the closest competitor where relevant.

Why explain features: The product page already lists features. The reviewer's job is to explain which features matter in practice and which are marketing bullet points. "256GB storage" is a spec. "256GB is enough for ~50,000 photos or ~100 apps — comfortable for most users, tight for power users who shoot 4K video" is a review.

**Performance and Experience (3-5 paragraphs):**
- This is the core of the review — what is it actually like to use this product?
- Report real-world usage, not just benchmark numbers.
- Include specific scenarios: "During a 3-hour flight, I edited 20 photos and the battery dropped to 62%."
- Note anything that surprised you — positively or negatively.

Why real-world matters: Benchmarks measure capability; usage reveals experience. A laptop can have the fastest processor in its class and still frustrate users with a terrible trackpad. Only hands-on experience surfaces these mismatches.

**Verdict (1-2 paragraphs):**
- Synthesize your assessment into a clear recommendation.
- State who should buy this and who should look elsewhere.
- Name the single best alternative for readers who decide against it.
- If using a numerical rating, state it here with a one-sentence justification.

---

## Evidence-Based Claims

Every evaluative claim in a review must be traceable to evidence. This is the difference between a review and an opinion.

### Types of Evidence

**Direct experience.** "The keyboard feels mushy after extended typing sessions" — this is a first-person observation that the reader can weigh against the reviewer's credibility.

**Measurements and data.** "Battery lasted 11 hours 23 minutes in our video playback test" — quantifiable, reproducible, comparable.

**Comparative observation.** "The autofocus is noticeably faster than the previous model, but still trails the competitor by a visible margin in low light" — grounded in specific comparison points.

**User reports and consensus.** "Multiple users on the support forum report the same Bluetooth disconnection issue" — citing community evidence for issues the reviewer may not have personally encountered.

### Claim Discipline

**Never write "it's fast" without context.** Fast compared to what? Fast at which task? "The M3 Pro renders a 10-minute 4K video in 4 minutes — roughly twice as fast as the Intel i7 equivalent" is an evidence-based claim. "It's really fast" is a feeling.

**Distinguish objective from subjective.** "The display measures 500 nits peak brightness" is objective. "The display looks vibrant in direct sunlight" is subjective but grounded in a specific scenario. "The display is beautiful" is purely subjective and provides no useful information.

**Qualify time-bound claims.** Software updates can change performance, features, and reliability. State the version or date: "As of firmware 2.1, the noise cancellation is effective but introduces slight hiss at low volumes." This protects the review's accuracy over time.

---

## Comparison Frameworks

When reviewing multiple products against each other, structure matters more than in a single-product review.

### The Matrix Approach

For direct comparisons of 3+ products, a table communicates more efficiently than prose.

**Why tables work for comparisons:** A reader comparing three laptops needs to see RAM, storage, weight, and battery side by side. Describing each laptop's specs in separate paragraphs forces the reader to hold information in working memory and mentally construct the comparison. A table does this work for them.

**Comparison table principles:**
- Include only criteria that differ meaningfully between products. If all three have USB-C, omitting that row is cleaner.
- Put the recommended option first (or highlight it) — make your recommendation visible in the table, not just the prose.
- Include price as a row. Every comparison is ultimately a value assessment.

### The Use-Case Frame

When products serve different audiences, compare them by use case rather than spec-by-spec.

"For video editing, Product A wins on raw render speed. For photography, Product B's display accuracy is unmatched. For general productivity at a budget, Product C offers 90% of the performance at 60% of the price."

Why this works: Readers don't buy specs — they buy solutions to their problems. Framing comparisons by use case directly addresses the reader's decision: "Which one is best *for what I need*?"

---

## Avoiding Bias

### Disclosure

If you received the product for free, have a relationship with the manufacturer, or have any financial interest in the review's outcome, disclose it. Not because readers always care, but because the one time they discover undisclosed bias is the last time they trust you.

**Where to disclose:** At the top of the review, before the first evaluative statement. Not buried in a footnote. Not at the end. The reader should know the context before processing your opinions.

### Recency Bias

The most recently used product feels best because the experience is freshest. Combat this by:
- Taking notes during the entire review period, not just at the end
- Re-evaluating first impressions after extended use — a product that wows on day one may frustrate by day seven
- Comparing to documented notes from previous reviews, not just memory

### Confirmation Bias

If you expected a product to be good (or bad) before reviewing it, you will unconsciously filter evidence to confirm that expectation. Combat this by:
- Writing the pros section before checking whether you've been generous enough to the cons
- Actively searching for evidence that contradicts your initial impression
- Asking: "If this product were from an unknown brand, would I evaluate this feature the same way?"

### Brand Halo Effect

A strong brand reputation can inflate ratings. A company with a history of great products gets the benefit of the doubt on a mediocre one. Evaluate each product on its own merits, not on the brand's track record.

---

## The keySummary for Reviews

The keySummary is the review's title. It should tell the reader what product is being reviewed and signal the reviewer's assessment.

**Effective patterns:**
- Product + verdict: "iPhone 16 Pro Review: The Camera King Stumbles on Battery Life"
- Product + positioning: "Framework Laptop 16: The Right-to-Repair Champion Grows Up"
- Comparison + winner: "M3 MacBook Air vs. ThinkPad X1 Carbon: Which Ultrabook Wins for Developers?"

**Anti-patterns:**
- Generic: "My Thoughts on the New Laptop" (which laptop? what thoughts?)
- Score-only: "iPhone 16 Pro: 8/10" (the number means nothing without context)
- Unqualified superlative: "The Best Phone Ever Made" (ever? really? by what criteria?)

---

## References

For deeper exploration of specific topics:

- **[Balanced Assessment](references/balanced-assessment.md)** — Objectivity techniques, testing methodology, and comparison matrices for structured evaluation.
