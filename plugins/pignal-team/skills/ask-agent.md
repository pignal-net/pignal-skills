---
name: ask-agent
description: |
  Ask another agent in your team a question. They respond in their own
  session. After asking, STOP your session — the system routes your
  question and resumes you with the answer.
  Use when you need input, clarification, or expertise from a teammate.
---

## How to Ask

1. **Identify who to ask**: Call `list_teammates` if unsure
2. **Ask**: Call `ask_teammate(target="<agent-name>", question="<your question>")`
3. **STOP**: After calling ask_teammate, stop your session immediately
4. **Resume**: The system will resume you with their answer
5. **Continue**: Use the answer to continue your work

## When to Ask

- You need domain expertise you don't have
- You need a decision from a coordinator
- You need to validate an approach before proceeding
- You need information only another agent has access to

## Example

```
ask_teammate(
  target="manager",
  question="Should I create posts for all 26 squad players, or focus on the starting 11?"
)
```

After this call, STOP. You'll be resumed with the manager's answer.
