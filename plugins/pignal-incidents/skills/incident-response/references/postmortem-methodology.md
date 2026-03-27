# Postmortem Methodology — Deep Dive

## Building a Blameless Culture

### The Cultural Foundation

Blameless postmortems cannot be mandated — they must be modeled. If leadership publicly blames individuals after incidents, no amount of process documentation will convince engineers to be candid. Culture is what happens when the document is closed.

### Psychological Safety in Practice

**The test:** Can a junior engineer say "I deployed the change that caused the outage" in a room full of senior staff without fearing consequences? If the answer is no, the culture is not blameless, regardless of what the process document says.

**How to build it:**
- Leaders go first. Senior engineers share their own mistakes openly.
- Praise candor explicitly. "Thank you for being transparent about what happened" should be a regular phrase in reviews.
- Separate the incident review from performance reviews. If postmortem content can affect promotions or compensation, people will self-censor.
- Celebrate near-misses. An incident that was caught before it affected users is a success story, not a failure to be hidden.

### When the Culture Is Not Ready

If you are in an organization that still blames, you cannot fix the culture in a single postmortem. What you can do:
- Write the document in blameless language (system states, not moral judgments).
- Focus on systemic causes rather than individual actions.
- Frame action items as system improvements, not behavioral corrections.
- Over time, the quality of learning will speak for itself.

## Facilitating the Post-Incident Review

### Before the Meeting

- Circulate the draft postmortem document 24 hours before the review.
- Ask participants to add their perspective to the timeline.
- Set ground rules: no blame, no interrupting, questions before judgments.

### During the Meeting

**The facilitator's role:** The facilitator is not the author, the incident commander, or the person who caused the incident. They are a neutral party whose job is to keep the discussion productive.

**Opening:** Restate the ground rules. Remind everyone that the goal is learning, not judgment. Read the timeline aloud — this creates a shared starting point.

**Timeline review:** Walk through the timeline chronologically. At each entry, ask:
- "Does anyone remember something different?"
- "What information did you have at this point?"
- "What were you thinking when this happened?"

These questions surface the gap between what people knew in the moment and what is obvious in hindsight. That gap is where the learning lives.

**Root cause discussion:** Present the 5 Whys chain. Ask:
- "Did we go deep enough?"
- "Are there alternative causal chains we should explore?"
- "What assumptions are embedded in this analysis?"

**Action item generation:** For each finding, ask: "What specific, bounded change would address this?" Resist the urge to generate action items for everything — prioritize the highest-leverage improvements.

### After the Meeting

- Update the postmortem document with meeting findings within 24 hours.
- Assign owners and due dates to all action items.
- Publish the final postmortem for organizational access.
- Schedule a 30-day follow-up to check action item progress.

## Action Item Tracking

### Why Action Items Decay

Action items from postmortems have a half-life. In the week after an incident, motivation is high. A month later, the next sprint's features have crowded out the improvements. Three months later, the action items are forgotten.

**The fix:** Track postmortem action items in the same system as feature work. If they are in a separate document, they are invisible. If they are tickets in the backlog, they compete for attention — which is exactly where they belong.

### Prioritizing Action Items

Not all action items are equal. Prioritize by:

**Likelihood of recurrence** — How probable is it that this exact failure mode will repeat? A configuration that silently breaks on every deploy needs fixing before a race condition that has happened once in two years.

**Blast radius** — If the failure recurs, how many users are affected? A fix that protects all users from a common failure mode is higher priority than one that protects a small subset from a rare one.

**Implementation cost** — A 2-hour fix that prevents recurrence is almost always worth doing immediately. A 2-month project needs to be weighed against other priorities.

### The Follow-Up Review

Schedule a review 30 days after every Sev 1 and Sev 2 incident:
- Are all action items completed or on track?
- Has the fix been verified to work? (Deploy the fix, then test the failure mode.)
- Has a similar incident occurred since? If so, were the fixes effective?

## Learning Reviews

### Beyond Individual Incidents

Once you have accumulated 10-20 postmortems, patterns emerge that are invisible in individual reviews. Conduct quarterly learning reviews that analyze incidents as a set:

**Common root causes:** Are most incidents caused by deployment failures? Configuration errors? Third-party dependencies? The answer determines where systemic investment will have the most impact.

**Detection patterns:** How are incidents being found? If most are reported by users rather than monitoring, the alerting system needs investment.

**Response patterns:** Are the same teams always responding? Is response time improving or degrading? Are runbooks being used?

**Action item completion rate:** What percentage of action items are completed within their deadline? If the rate is below 70%, either the items are too large, too low-priority, or the tracking system is failing.

### Sharing Across Teams

Postmortems are most valuable when they cross team boundaries. An incident in the payment system may have lessons for the authentication team. Publish postmortems broadly and summarize key learnings in a monthly digest.

**The format that works:** A monthly email or Slack post with:
- One-sentence summary of each incident
- The most interesting or transferable lesson from each
- Links to the full postmortems for anyone who wants depth

This is low-effort to produce and high-value for the organization. Most people will read the summaries; the few who need depth will follow the links.

## Incident Classification Calibration

### The Calibration Problem

Without calibration, different teams classify the same incident differently. Team A calls their Sev 2 what Team B would call a Sev 3. This makes aggregate metrics meaningless and creates inconsistent response expectations.

### Calibration Sessions

Quarterly, bring incident commanders and on-call leads together for a calibration session:
1. Present 5-10 past incidents with severity stripped out.
2. Each participant independently classifies them.
3. Discuss disagreements — these are where the classification criteria need clarification.
4. Update the severity definitions based on the discussion.

### Edge Cases Worth Discussing

- **Brief but total outage:** 30 seconds of complete downtime — Sev 1 (total loss of service) or Sev 3 (brief, limited impact)?
- **Slow degradation:** p99 latency increased 10x over 4 hours. No errors, but user experience is poor.
- **Internal-only impact:** A developer tool is completely down. No external user impact, but engineering productivity is halted.
- **Near-miss:** A dangerous condition was detected and mitigated before any impact. Sev 4, or not an incident at all?

These discussions build shared understanding. The specific answers matter less than the consistency they produce.
