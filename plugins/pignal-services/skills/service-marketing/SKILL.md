---
name: service-marketing
description: |
  Service marketing craft for Pignal Service Directory sites. Service
  positioning, tier design, availability communication, and value
  differentiation. Use when listing services, creating pricing tiers,
  managing service offerings, or operating a Pignal services site.
  Also use for professional service descriptions, consulting packages,
  or freelance service pages.
allowed-tools: [mcp__*]
---

# Service Marketing Craft

Services are harder to sell than products because they're intangible — customers can't hold them, try them, or return them. The craft of service marketing is making the invisible visible: clearly communicating what the customer gets, why it's valuable, and what happens after they say yes.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (service categories), workspaces (service areas), and limits

## Service Positioning

### Lead with the Outcome, Not the Process

Customers don't buy your time — they buy the result of your time.

- **Process-focused:** "We conduct 20 hours of market research and deliver a 50-page report"
- **Outcome-focused:** "We identify your most profitable customer segment so you can focus your marketing budget where it matters"

**Why this matters:** Process descriptions feel like cost justification. Outcome descriptions feel like value creation.

### Differentiation

Every service page must answer: "Why you instead of someone else?"

Differentiation comes from:
- **Specialization:** "We only work with SaaS companies" (narrower = more trusted)
- **Process:** "We use a proprietary 5-step framework" (unique methodology)
- **Proof:** "98% client retention rate" or "Helped 200+ companies" (evidence)
- **Speed:** "Results in 2 weeks, not 2 months" (time advantage)

## Deliverable Clarity

### Be Specific About What's Included

Vague service descriptions create scope disputes and disappointed customers.

- **Vague:** "We'll redesign your website"
- **Clear:** "You'll receive: new homepage design, up to 5 inner page templates, mobile-responsive implementation, and 2 rounds of revisions"

### The Deliverable Checklist

For each service, clearly communicate:
1. **What they get** — tangible outputs (designs, reports, code, documents)
2. **What happens** — the process/timeline (discovery call, 2-week sprint, review)
3. **What's not included** — boundaries prevent scope creep and set expectations
4. **What happens next** — the follow-up (support period, maintenance, next steps)

## Tier Design

### The Good/Better/Best Strategy

Three tiers work because they give customers agency without overwhelm:
- **Good (entry):** Solves the core problem, accessible price point
- **Better (target):** Most popular — includes the features most customers actually need
- **Best (premium):** Full solution for customers who want everything

**Why three tiers work:** The middle tier becomes the anchor. Most customers choose it because it feels like the "reasonable" choice — not too cheap (risky), not too expensive (excessive).

### Tier Naming

Names should reflect value, not just size:
- "Starter / Professional / Enterprise" (scale-oriented)
- "Essential / Growth / Scale" (outcome-oriented)
- Avoid "Basic" — it sounds inferior and discourages purchase

### Feature Differentiation Between Tiers

Each tier should add 2-3 clear benefits over the previous one. If you can't articulate the difference, the tiers aren't distinct enough.

## Availability Communication

### Current Status

Be explicit about availability:
- "Currently accepting new clients" — reduces hesitation
- "Booked through [month] — join the waitlist" — creates urgency and signals demand
- "Limited to 3 new clients per month" — scarcity without dishonesty

### Response Time Expectations

Set expectations for the inquiry-to-engagement timeline:
- "We respond to inquiries within 24 hours"
- "Typical project kickoff is 2 weeks from signing"

**Why this matters:** Silence after an inquiry kills conversion. Even if you're busy, tell them when to expect a response.

## Pricing Justification

### Value Framing

Frame price in terms of return:
- "This $5,000 audit typically identifies $20,000+ in wasted ad spend"
- "Our $200/month retainer replaces a $60,000/year hire"

### Social Proof

Testimonials are more convincing for services than for products because services require trust. Place proof close to the pricing:
- Client logos
- Specific results ("Increased conversion by 34%")
- Testimonial quotes

## Common Mistakes

- **Listing features without benefits:** "Includes 4 strategy sessions" → "4 strategy sessions to align your team on priorities and eliminate wasted effort"
- **No call to action:** Every service page needs a clear next step (book a call, request a quote, start a trial)
- **Generic descriptions:** "We provide high-quality solutions" says nothing. Be specific about what you do and for whom.
- **Hiding pricing:** If you can't list exact prices, give ranges or "starting at" figures. "Contact us for pricing" signals either expensive or disorganized.

## Workflow

1. **Discover** — `get_metadata` for types, workspaces, limits
2. **Position** — Define each service's outcome and differentiation
3. **Structure** — `save_item` with outcome-first description, clear deliverables
4. **Tier** — Use types or tags to distinguish service levels
5. **Proof** — Add testimonials, metrics, client logos in content
6. **Publish** — `validate_item` then `vouch_item`
