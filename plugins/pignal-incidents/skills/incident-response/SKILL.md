---
name: incident-response
description: |
  Incident communication craft for Pignal Incident Log sites. Severity
  classification, timeline construction, root cause analysis, and blameless
  postmortem methodology. Use when documenting incidents, writing postmortems,
  managing incident logs, or communicating outages.
allowed-tools: [mcp__*]
---

# Incident Communication Craft

An incident log serves two audiences: the people currently fighting the fire, and the people who will learn from it afterward. The craft is in writing for both simultaneously — clear enough to coordinate a response, honest enough to produce genuine learning.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (incident categories), workspaces (services/teams), and limits

---

## Severity Classification

Severity is not a measure of how bad something feels — it is a structured assessment of user impact, scope, and duration. Consistent classification matters because it determines who gets paged, what communication channels activate, and how urgently the response proceeds.

### Why Severity Must Be Defined Before Incidents Happen

During an incident, adrenaline distorts judgment. A database that is slow but functional can feel catastrophic at 2 AM. A complete outage of a low-traffic internal tool can feel minor because nobody is complaining yet. Pre-defined severity levels remove judgment from the moment when judgment is most impaired.

### Severity Levels

**Critical (Sev 1)** — Complete loss of service for external users, data loss or corruption, security breach with active exploitation. The defining characteristic: users cannot accomplish their primary task at all. Revenue or safety is directly affected.

**Major (Sev 2)** — Significant degradation of service. Core functionality works but is severely impaired — high error rates, extreme latency, partial outages affecting a segment of users. The defining characteristic: users can work around it, but the workaround is painful or temporary.

**Minor (Sev 3)** — Limited impact. A non-critical feature is broken, a subset of users is affected, or the issue is intermittent. The defining characteristic: most users are unaffected or unaware.

**Informational (Sev 4)** — No current user impact but a condition exists that could escalate. A monitoring alert that hasn't manifested as user-visible yet. The defining characteristic: proactive, not reactive.

### Classification Principles

**Classify by impact, not by cause.** A trivial code change that causes a Sev 1 is still a Sev 1. A sophisticated attack that causes a Sev 3 is still a Sev 3. The response is determined by the effect on users, not the technical sophistication of the problem.

**Severity can change.** Start with your best assessment and reclassify as you learn more. A Sev 3 that turns out to affect more users than initially thought becomes a Sev 2. Document the reclassification in the timeline — it is useful learning, not a mistake.

**When in doubt, classify higher.** It is less costly to over-classify (page too many people, communicate too broadly) than to under-classify (miss the response window, surprise stakeholders later).

---

## Timeline Construction

The timeline is the spine of every incident document. It transforms a chaotic event into a legible sequence that humans can reason about.

### Why Timelines Are Non-Negotiable

Without a timeline, incident reviews devolve into competing narratives. Person A remembers one sequence, Person B remembers another. The timeline is the shared source of truth that makes productive discussion possible.

### Timeline Principles

**Timestamps are mandatory.** Every entry needs a time, in a consistent timezone. "Around lunchtime" is not a timeline entry. "12:34 UTC" is.

**Record what happened, not what should have happened.** "12:47 — Engineer noticed the alert had been firing for 13 minutes" is more useful than "12:47 — Alert acknowledged." The gap between the alert firing and the human noticing it is information that drives improvement.

**Include negative events.** "13:05 — Attempted rollback. Rollback failed because the previous version had been garbage-collected." Failures during response are often the most valuable learning.

**Separate observation from interpretation.** "13:12 — Error rate dropped to 0.1% (from 45%)" is an observation. "13:12 — The fix worked" is an interpretation. Record observations; save interpretation for the analysis section.

### Timeline Structure

```
11:42 UTC — Monitoring alert: API error rate exceeds 10% threshold
11:45 UTC — On-call engineer acknowledges alert, begins investigation
11:52 UTC — Root cause identified: database connection pool exhausted
11:55 UTC — Mitigation attempted: restart API pods
11:57 UTC — Error rate unchanged after pod restart
12:03 UTC — Connection pool limit increased from 20 to 50
12:05 UTC — Error rate begins declining
12:12 UTC — Error rate returns to baseline (<0.1%)
12:15 UTC — Monitoring confirms stable for 10 minutes, incident closed
```

---

## Root Cause Analysis

### The 5 Whys

Root cause analysis asks "why" repeatedly until you reach a cause that is systemic rather than proximate. The technique is deceptively simple, which is why it is so often done poorly.

**Why it works:** Human instinct is to stop at the first "why." The database ran out of connections — that is the root cause, right? But *why* did the database run out of connections? Because a new feature opened connections without closing them. But *why* did that reach production? Because the code review didn't catch it. But *why* didn't the code review catch it? Because there is no linting rule or test for connection leaks. Now you are at a systemic cause — and the fix (add a linting rule) prevents an entire class of future incidents, not just this one.

**Common traps:**

- **Stopping too early.** If your root cause is "a human made a mistake," you have not gone deep enough. Humans always make mistakes. The system must tolerate them.
- **Branching too wide.** Each "why" should follow the primary causal chain, not enumerate every contributing factor. Save contributing factors for a separate section.
- **Confusing correlation with causation.** "We deployed at 11:30 and the incident started at 11:42" is correlation. You need to establish the causal mechanism.

### Contributing Factors

Most incidents have one root cause and multiple contributing factors. Contributing factors did not cause the incident alone but made it more likely, harder to detect, or slower to resolve.

**Detection gaps:** Why did it take 13 minutes to notice the alert? Was the on-call engineer in a meeting? Was the alert routed to a noisy channel?

**Response friction:** Why did the first mitigation fail? Was the rollback process untested? Was documentation outdated?

**Amplifiers:** What made the impact worse than it should have been? Was there a thundering herd? Did a retry storm amplify the original failure?

---

## Blameless Postmortems

### Why Blame Kills Learning

This is the single most important principle in incident management, and the one most frequently violated.

**The psychological argument:** When people fear punishment, they hide information. They omit details from timelines, sanitize their actions, and avoid admitting what they did not know. The incident review loses access to exactly the information it needs most — what actually happened and why.

**The systems argument:** If a single engineer's mistake can cause an outage, the problem is not the engineer — it is the system that allowed one mistake to have that impact. Blaming the individual treats the symptom. Fixing the system treats the disease. Every "human error" is a system design error in disguise.

**The statistical argument:** The engineer who caused today's incident is the same one who prevented yesterday's incident. Punishing errors does not eliminate them — it drives them underground. Organizations that blame have the same error rate as organizations that don't. They just have worse data about their errors.

### Blameless Does Not Mean Accountable-less

Blameless means we do not ask "whose fault was this?" It does not mean we avoid asking "what happened and what should change?" People are still responsible for following up on action items, implementing fixes, and improving their skills. The distinction is between accountability (forward-looking: what will you do differently?) and blame (backward-looking: who should be punished?).

### Language Patterns

| Blaming | Blameless |
|---------|-----------|
| "John forgot to check the runbook" | "The runbook was not referenced during response" |
| "The team should have known better" | "The team's mental model did not include this failure mode" |
| "This was caused by a careless deploy" | "The deploy process did not catch the regression" |
| "QA missed this bug" | "This failure mode was not covered by existing tests" |

The blameless versions are not euphemisms — they are more *accurate*. They describe system states rather than moral judgments, which makes them actionable.

---

## Action Items

Action items are the entire point of incident reviews. If an incident review produces no action items, it was a storytelling exercise, not a learning exercise.

### Three Types of Action Items

**Preventive** — Eliminate the root cause so this specific failure cannot recur.
- Example: "Add connection pool leak detection to the CI pipeline"
- Why: Prevents the exact same incident from happening again.

**Detective** — Improve the ability to detect similar failures earlier.
- Example: "Add an alert for connection pool utilization above 80%"
- Why: The next incident with a similar pattern will be caught sooner, reducing impact.

**Corrective** — Improve the ability to respond and recover from similar failures faster.
- Example: "Document the connection pool scaling procedure in the runbook"
- Why: When a similar incident occurs, the response will be faster and more reliable.

### Action Item Quality

**Specific and assignable.** "Improve monitoring" is not an action item. "Add a Datadog alert for API p99 latency exceeding 500ms, assigned to @platform-team, due 2024-02-15" is an action item.

**Bounded in scope.** If an action item takes more than two weeks, it is a project, not an action item. Break it down.

**Tracked to completion.** An action item without a due date and an owner is a wish. Track them in a system that surfaces incomplete items.

---

## Status Page Communication

When an incident is in progress, external communication must be clear, honest, and timely. Users do not need technical details — they need to know three things: what is broken, whether you know about it, and when they can expect an update.

### Communication Principles

**Acknowledge fast, even without details.** "We are investigating reports of increased error rates" posted within 5 minutes is better than a detailed technical explanation posted after 30 minutes. Users can tolerate problems; they cannot tolerate silence.

**Be specific about impact.** "Some users may experience issues" is useless. "Users in the EU region are unable to log in" tells affected users they are not alone and unaffected users they can stop worrying.

**Commit to update intervals.** "We will provide an update within 30 minutes" sets expectations. Then meet that commitment — even if the update is "still investigating, next update in 30 minutes."

**Separate what you know from what you are doing.** "We have identified the cause as a database issue (what we know). We are working on scaling the database cluster (what we are doing). We expect resolution within 1 hour (when we expect it fixed)."

### Status Page Sequence

1. **Investigating** — We know something is wrong, we are looking into it.
2. **Identified** — We know what is wrong and are working on a fix.
3. **Monitoring** — The fix is deployed, we are watching for recurrence.
4. **Resolved** — Service is fully restored. Brief summary of what happened and duration.

---

## Post-Incident Review Structure

The written postmortem document captures the full learning from the incident. Structure it for readability — someone who was not involved should be able to understand the incident, its causes, and its lessons from this document alone.

### Document Sections

1. **Summary** — One paragraph: what happened, when, how long, who was affected, severity.
2. **Impact** — Quantified: number of users affected, duration of impact, error rates, revenue impact if applicable.
3. **Timeline** — Complete chronological sequence (see "Timeline Construction").
4. **Root Cause** — The 5 Whys chain, clearly stated.
5. **Contributing Factors** — What made detection, response, or recovery harder.
6. **What Went Well** — What worked during the response. This section prevents postmortems from becoming purely negative.
7. **What Went Poorly** — Where the response fell short.
8. **Action Items** — Preventive, detective, and corrective items with owners and due dates.
9. **Lessons Learned** — Broader takeaways that apply beyond this specific incident.

---

## Incident Metrics

Metrics turn individual incidents into trend data. Track these consistently across all incidents.

**MTTD (Mean Time to Detect)** — Time from incident start to first human awareness. This measures monitoring effectiveness. If MTTD is consistently high, your alerting has blind spots.

**MTTR (Mean Time to Resolve)** — Time from first human awareness to full resolution. This measures response capability. If MTTR is high but MTTD is low, you detect fast but respond slowly — invest in runbooks and tooling.

**MTTA (Mean Time to Acknowledge)** — Time from alert firing to human acknowledgment. The gap between MTTD and MTTA reveals whether alerts are reaching the right people at the right time.

**Recurrence rate** — How often do you have incidents with the same root cause? A non-zero recurrence rate means action items are not being completed or are not effective.

---

## Workflow

1. **Discover** — `get_site_tools` then `get_metadata` for incident types and workspaces
2. **Classify** — Assign severity based on user impact, not technical cause
3. **Document** — `save_item` with timeline, root cause, and action items
4. **Review** — `validate_item` to mark as reviewed (indicates postmortem is complete)
5. **Publish** — `vouch_item` for organizational transparency and learning

For deeper guidance on blameless culture and facilitation, see [postmortem-methodology.md](references/postmortem-methodology.md).
