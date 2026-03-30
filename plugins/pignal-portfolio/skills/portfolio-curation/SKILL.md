---
name: portfolio-curation
description: |
  Portfolio presentation craft for Pignal Portfolio sites. Project
  storytelling, visual hierarchy, impact quantification, and portfolio
  sequencing. Use when showcasing projects, adding portfolio entries,
  or presenting creative work.
allowed-tools: [mcp__*]
---

# Portfolio Presentation Craft

A portfolio is not a list of things you have done — it is an argument for what you are capable of doing next. Every project entry must answer the viewer's unspoken question: "If I hire this person or engage this team, what will they do for me?" The craft is in selecting, framing, and sequencing work so that argument is compelling.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (project categories), workspaces (disciplines/domains), and limits

---

## The Challenge-Approach-Outcome Framework

Every portfolio entry tells a story. The most effective structure is: what was the problem, how did you solve it, and what happened as a result. This framework works because it mirrors how decision-makers evaluate capability — they want to see that you can diagnose problems, devise solutions, and deliver results.

### Challenge

Start with the problem, not the solution. Describe the situation before your involvement — what was broken, missing, constrained, or needed. The challenge section establishes stakes. Without stakes, the reader has no reason to care about your solution.

**Why the challenge comes first:** Showing the solution without the problem is like showing a punchline without a setup. The viewer cannot appreciate the difficulty of what you built unless they first understand what made it difficult. A beautifully designed interface is less impressive in a vacuum than when the viewer knows the constraint was "500ms load time budget on 3G connections in rural India."

**Principles:**
- Be specific about constraints. "The client wanted a website" is not a challenge. "The client needed to increase online bookings by 40% within 3 months, with no budget for paid advertising" is a challenge.
- Name the stakeholder. Who had the problem? What was at stake for them?
- Quantify the starting state when possible. "Conversion rate was 1.2%" gives the outcome something to be measured against.

### Approach

Describe what you did and — critically — why you chose that approach over alternatives. The approach section demonstrates judgment, not just execution. Anyone can describe what they built. The differentiator is showing *why* they built it that way.

**Why alternatives matter:** Stating your approach without mentioning alternatives suggests you only know one way to solve problems. "We chose server-side rendering because initial load performance was the primary conversion driver, and the content was largely static" demonstrates that you evaluated options and selected based on evidence.

**Principles:**
- Lead with the strategic decision, not the implementation details. "We restructured the information architecture around user tasks rather than organizational departments" before "We used React and Next.js."
- Describe your role explicitly. "I led the user research and designed the interaction patterns" is more credible than "We did user research."
- Include one meaningful technical or creative decision. Not every decision — just the one that best demonstrates expertise.

### Outcome

Results are the point. Everything before this section is context for this section. Outcomes must be specific, measurable, and honestly attributed.

**Principles:**
- Use real numbers. "Improved significantly" is not an outcome. "Conversion rate increased from 1.2% to 3.8% (217% improvement)" is an outcome.
- Attribute honestly. If the outcome was influenced by factors beyond your work (a marketing campaign launched simultaneously, market conditions shifted), acknowledge that. Overclaiming destroys credibility when discovered.
- Include timeline. "Within 6 weeks of launch" grounds the outcome in reality.

---

## Visual Hierarchy

Portfolio viewers scan before they read. Visual hierarchy determines what they notice first, what holds their attention, and what they skip. Since Pignal sites use markdown content, visual hierarchy is achieved through content structure, image placement, and writing density.

### The Scan Path

Viewers follow a predictable pattern: hero image or screenshot first, then title, then the first sentence of the description. If those three elements do not communicate the project's essence, the viewer moves on. They never reach your carefully crafted approach section.

**Why this matters for content structure:** Place the most visually compelling asset at the top. Write the keySummary (title) to communicate the project type and domain immediately. Write the first paragraph to answer "what is this and why should I care?" — everything else comes after.

### Image Strategy in Markdown

- **Lead with one strong image.** The first image in the content is the hero. Make it the most representative view of the finished work.
- **Show context, not just output.** A screenshot of a mobile app is good. A mockup showing the app in a hand, in a realistic context, is better — it helps the viewer imagine the work in the real world.
- **Before/after pairs for redesigns.** Place them side by side (using markdown layout) or in sequence with clear labels. The contrast does the persuasion.
- **Process images are optional.** Wireframes, sketches, and prototypes are interesting to other practitioners but rarely to hiring managers or clients. Include them if your audience is other designers/developers; omit them if your audience is decision-makers.

### Content Density

**Short paragraphs.** Portfolio text competes with visuals for attention. Long paragraphs lose that competition. Keep paragraphs to 2-3 sentences.

**Section headers as navigation.** Use headers (Challenge, Approach, Outcome, or domain-appropriate equivalents) so scanning viewers can jump to what interests them.

**Bold key phrases.** The first clause of important sentences can be bolded to create a scannable layer within the text. Viewers who only read bold text should still get the core story.

---

## Portfolio Sequencing

The order of projects in a portfolio is itself an argument. It is not chronological — it is strategic.

### The Primacy-Recency Effect

Psychological research consistently shows that people remember the first and last items in a sequence better than the middle items. This is the serial position effect (Ebbinghaus, 1885, replicated extensively since). Applied to portfolios:

**First position:** Your strongest, most relevant piece. This sets the viewer's expectations for everything that follows. If the first project impresses, they evaluate subsequent projects generously. If it disappoints, they scan the rest with skepticism.

**Last position:** Your second-strongest piece, or the piece that best represents where you are heading. The last impression lingers — it is what the viewer carries into their evaluation.

**Middle positions:** Solid work that demonstrates range, consistency, or specific skills. The middle is where you show breadth without risking boredom.

### Sequencing Strategies

**For job seekers:** Lead with work most relevant to the target role. If applying for a product design position, the first project should be product design, even if your best visual design work is more impressive in isolation.

**For freelancers/agencies:** Lead with work in the prospect's industry. A SaaS company looking at your portfolio wants to see SaaS work first, not your award-winning restaurant branding.

**For career changers:** Lead with transferable work that bridges old and new domains. If transitioning from print to digital, lead with print projects that demonstrate digital-relevant skills (information architecture, user-centered thinking).

### How Many Projects

**Quality over quantity, always.** Five strong projects beat twenty mediocre ones. A portfolio with weak entries tells the viewer "this person cannot distinguish their good work from their bad work" — which is a damaging signal about judgment.

**The sweet spot:** 4-8 projects for most portfolios. Enough to demonstrate range, not so many that the viewer faces decision fatigue.

**Retire old work.** Projects from more than 3-5 years ago should be evaluated harshly. Does this still represent your current skill level? If not, remove it. An outdated project actively undermines the impression created by your current work.

---

## NDA-Safe Descriptions

Much of the best work happens under confidentiality agreements. This is not a barrier to portfolio inclusion — it is a writing challenge.

### Strategies for Confidential Work

**Describe the problem class, not the client.** "A Fortune 500 financial services company needed to reduce customer onboarding time" communicates the domain, scale, and challenge without naming the client.

**Generalize the solution.** "We redesigned the multi-step form flow, reducing fields from 47 to 12 by using progressive disclosure and API-based auto-fill" describes your approach without revealing proprietary business logic.

**Quantify with ratios, not absolutes.** "Reduced onboarding time by 60%" is usually safe when "Reduced onboarding from 23 minutes to 9 minutes for 2.3M monthly users" might not be.

**Use process artifacts.** Wireframes, user flows, and system diagrams can often be shared when final screenshots cannot. Annotate them to show your thinking.

**Ask for permission.** Many clients will allow portfolio inclusion with approval. Ask, and get it in writing. A portfolio entry with the client's name and logo is always more credible than an anonymized one.

---

## Impact Quantification

Numbers are the most credible form of evidence in a portfolio. But not all numbers are equally persuasive — and fabricated or inflated numbers are career-ending when discovered.

### Metric Selection

**Business metrics over vanity metrics.** "Increased revenue by $120K/month" is more meaningful than "Increased page views by 300%." Page views do not pay salaries.

**Relative metrics need baselines.** "Improved performance by 40%" only means something if the reader knows the starting point. "Reduced page load time from 8.2s to 1.4s (83% improvement)" tells the full story.

**User metrics over technical metrics.** "Task completion rate increased from 54% to 91%" resonates more than "Reduced API response time by 200ms" — even though the latter caused the former. Lead with what users experienced.

### When You Do Not Have Numbers

Not all work produces measurable outcomes. Creative work, early-stage projects, and internal tools often lack quantitative results. In these cases:

- **Describe qualitative outcomes.** "The design system was adopted by 4 product teams within 2 months of launch" is credible without a revenue number.
- **Quote stakeholders.** "The VP of Product called it 'the single most impactful initiative this quarter'" is social proof.
- **Describe scope and constraints.** "Designed and shipped in 3 weeks with a team of 2" communicates capability even without outcome metrics.

---

## Tool and Technology Attribution

Mentioning tools demonstrates technical range, but tools should never be the focus of a portfolio entry. The viewer cares about what you accomplished, not what you used.

**The right placement:** A brief "Built with" line at the end of the entry, or tags. Not in the opening paragraph, not in the title.

**Why tools are secondary:** Tools change constantly. Figma replaces Sketch. Next.js replaces Gatsby. The problem-solving ability that chose the right tool is more durable and more valuable than familiarity with any specific tool.

**Exception:** When the tool choice *was* the insight. "Migrated from a monolithic CMS to a headless architecture, reducing deploy time from 45 minutes to 12 seconds" — here, the architectural decision is the story.

---

## The keySummary

The keySummary is the project title — the first text viewers see in the portfolio feed. It must communicate the project type and domain instantly.

**Good patterns:**
- "Redesigning Checkout for a B2B SaaS Platform"
- "Brand Identity System for Urban Agriculture Startup"
- "Real-Time Analytics Dashboard for IoT Fleet Management"

**Anti-patterns:**
- "Cool Project" (no information)
- "Client Work Q3 2024" (internal labeling, not public communication)
- "Figma / React / Node.js / AWS" (tool list, not project description)

---

## Workflow

1. **Discover** — `get_site_tools` then `get_metadata` for project types and workspaces
2. **Select** — Choose projects strategically (strongest first and last)
3. **Frame** — Structure each entry as challenge, approach, outcome
4. **Draft** — `save_item` with compelling keySummary and structured content
5. **Validate** — `validate_item` to mark as reviewed
6. **Publish** — `vouch_item` with a descriptive slug (e.g., `checkout-redesign-b2b-saas`)
