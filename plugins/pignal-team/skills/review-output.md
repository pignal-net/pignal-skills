---
name: review-output
description: |
  Review another agent's output for quality and completeness.
  Use when evaluating delegated work, checking quality before reporting,
  or deciding if additional work is needed.
---

## How to Review

When you receive output from a delegated agent, evaluate:

1. **Completeness** — Did they do everything asked?
2. **Quality** — Is the work professional and accurate?
3. **Correctness** — Are there factual errors or bugs?
4. **Intent** — Does the result match what was originally requested?

## Decision Framework

After review, choose one:

- **Accept** → Call `report_completed(status="completed", assessment="...")`
- **Needs more work** → Call `assign_to_teammate` again with specific feedback
- **Failed** → Call `report_completed(status="failed", assessment="...")`
- **Needs human review** → Call `report_completed(status="needs_review", assessment="...")`

## Tips

- Be specific in your assessment — what worked, what didn't
- If delegating again, include what needs to change
- Don't be overly critical of minor style differences
- Focus on whether the task intent was fulfilled
