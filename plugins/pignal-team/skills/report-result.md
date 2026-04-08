---
name: report-result
description: |
  Report a task result to the Pignal platform. Call when your work is
  complete, whether successful or failed. Always report — the platform
  needs to know the outcome.
---

## How to Report

Call `report_completed` with:
- **status**: "completed", "failed", or "needs_review"
- **assessment**: Your quality evaluation of the work
- **summary** (optional): Brief description of what was accomplished

## When to Report

- Task completed successfully → `status: "completed"`
- Task failed and cannot be recovered → `status: "failed"`
- Task partially done, needs human review → `status: "needs_review"`

Always report, even on failure. The platform tracks outcomes for
monitoring, billing, and quality metrics.

## Example

```
report_completed(
  status="completed",
  assessment="All 26 player profiles created with stats, bios, and photos. Quality is high — content is factual and well-structured.",
  summary="Created 26 World Cup 2026 player profile posts"
)
```

After reporting, STOP your session. The task is done.
