---
name: procedures
description: |
  Procedural writing craft for Pignal Runbook sites. SOP methodology,
  step-by-step instructions, escalation paths, and verification steps.
  Use this skill when creating runbooks, writing SOPs, documenting
  operational procedures, creating troubleshooting guides, or operating
  a Pignal runbook site. Also use for any step-by-step procedural
  documentation or operational guides.
allowed-tools: [mcp__*]
---

# Procedural Writing Craft

A runbook is a promise: "Follow these steps exactly and the outcome is predictable." The craft is in writing procedures so clear that someone unfamiliar with the system can execute them successfully under pressure.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (procedure categories), workspaces (systems/areas), and limits

## SOP Methodology

### Why Standard Procedures Reduce Errors

When humans operate under stress (incidents, time pressure, unfamiliar systems), they rely on working memory — which is limited and error-prone. Written procedures externalize memory, reducing cognitive load and ensuring nothing is missed.

The best runbooks are written for the worst moments — 3 AM, during an outage, by someone who hasn't touched this system in months.

### Imperative Voice

Use commands, not descriptions:
- **Good:** "Click the Deploy button"
- **Bad:** "The Deploy button should be clicked"
- **Bad:** "You'll want to click the Deploy button"

Imperative voice is unambiguous. Under pressure, people don't need conversational tone — they need clear instructions.

## Step Structure

### One Action Per Step

Each step should be a single, verifiable action:
- **Good:** "1. Open the AWS Console. 2. Navigate to EC2 > Instances."
- **Bad:** "1. Open the AWS Console and navigate to EC2 > Instances, then find the production server and check its status."

**Why:** When someone loses their place (phone rings, alert fires), they need to identify exactly which step they're on. Multi-action steps make that impossible.

### Expected Outcome Per Step

After each step, tell the reader what they should see:
```
3. Run `kubectl get pods -n production`
   Expected: All pods show STATUS "Running" and READY "1/1"
```

**Why:** Without expected outcomes, operators can't tell if a step succeeded. They proceed to the next step on faith, and problems compound.

### Decision Points

Some procedures branch. Make branching explicit:
```
5. Check the response code.
   - If 200: Continue to step 6
   - If 503: Go to "Service Unavailable Recovery" (page X)
   - If 401: Stop — escalate to the security team
```

### Time Estimates

Include time estimates for long steps:
```
7. Run the database migration (~5-10 minutes for production dataset)
```

**Why:** Without estimates, operators don't know if "still running" means "working normally" or "stuck." This prevents unnecessary escalation.

## Prerequisite Sections

Before the steps begin, list what must be true:
- Required access/permissions
- Required tools installed
- Required state of the system
- Required approvals

**Why:** Nothing is worse than reaching step 7 of 12 and discovering you need VPN access you don't have. Prerequisites prevent wasted effort.

## Escalation Paths

Define when to stop and call someone:
```
**Escalate if:**
- The migration has been running for more than 30 minutes
- You see errors containing "FATAL" or "PANIC"
- You're unsure about any step
```

**Why:** Clear escalation criteria prevent two failure modes: (1) escalating too early and wasting senior time, and (2) escalating too late and making the problem worse.

## Rollback Procedures

For any procedure that changes system state, include rollback steps:
```
**Rollback:**
If anything goes wrong after step 4, reverse the changes:
1. Run `kubectl rollout undo deployment/api -n production`
2. Verify rollback: `kubectl get pods -n production`
3. Expected: Previous version running, all pods healthy
```

**Why:** The ability to undo is the most important safety net. Without documented rollback, operators either guess (dangerous) or freeze (costly).

## Verification Steps

End every procedure with verification:
```
**Verify Success:**
1. Check health endpoint: `curl https://api.example.com/health`
   Expected: 200 OK with `{"status": "healthy"}`
2. Check monitoring dashboard: [link]
   Expected: No new alerts in the last 5 minutes
3. Spot-check a user action: [describe test]
```

## Testing Procedures

Runbooks should be tested, not just written:
- **Desk check:** Walk through mentally — does every step make sense?
- **Shadow run:** Someone follows the runbook while an expert watches
- **Solo run:** Someone follows the runbook independently
- **Periodic review:** Procedures drift as systems change — review quarterly

## Common Mistakes

- **Assuming knowledge:** "Configure the load balancer" — how? Be specific.
- **Missing error handling:** What if a step fails? Every step should address this.
- **Outdated screenshots:** Screenshots go stale fast. Prefer text descriptions.
- **No version/date:** Runbooks without dates can't be trusted. Always include "Last verified: YYYY-MM-DD."

## Workflow

1. **Discover** — `get_metadata` for procedure types and workspaces
2. **Structure** — Prerequisites, numbered steps, expected outcomes, rollback
3. **Draft** — `save_item` with imperative voice, private visibility
4. **Test** — Walk through the procedure (desk check minimum)
5. **Publish** — `validate_item` (marks as verified) then `vouch_item`

For deeper guidance, see [procedural-writing.md](references/procedural-writing.md).
