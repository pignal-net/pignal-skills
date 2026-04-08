---
name: delegate-task
description: |
  Delegate a task to another agent in your team. The target agent runs
  in its own isolated session. After delegating, STOP your session —
  the system will run the target agent and resume you with their output.
  Use when you need specialized work done by a team member.
---

## How to Delegate

1. **Check your team**: Call `pignal_team_info` to see available agents
2. **Choose the right agent**: Match the task to the specialist's expertise
3. **Write clear instructions**: The target agent has NO context from your session
4. **Delegate**: Call `pignal_delegate(target="<agent-name>", task="<instructions>")`
5. **STOP**: After calling pignal_delegate, stop your session immediately
6. **Resume**: The system will resume you with the target agent's output
7. **Review**: Evaluate the result and decide next steps

## Writing Good Delegation Prompts

Include in your instructions:
- What to do (the task)
- What context they need (site info, requirements, constraints)
- What "done" looks like (success criteria)
- Any specific requirements (format, quality level, tools to use)

The target agent is isolated — they can only see what you put in the prompt.

## Example

```
pignal_delegate(
  target="site-operator",
  task="Create 5 football player profile posts for the World Cup 2026 squad on the footballplayer site. Each post should include player stats, position, and a brief bio. Use the reviews template format."
)
```

After this call, STOP. You'll be resumed with the site-operator's output.
