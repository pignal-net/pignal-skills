# Procedural Writing — Deep Dive

## Clarity Techniques

### The Stranger Test

Write as if the reader has never seen the system before. If your procedure says "restart the service," a stranger won't know which service, on which server, using which command. Be explicit:

```
Restart the API service on the production server:
  ssh prod-api-1
  sudo systemctl restart api-server
```

### Eliminating Ambiguity

Ambiguous words in procedures are dangerous:

| Ambiguous | Clear |
|-----------|-------|
| "Wait a moment" | "Wait 30 seconds" |
| "Check that it's working" | "Verify the health endpoint returns 200" |
| "Update the configuration" | "Set `MAX_CONNECTIONS=100` in `/etc/app/config.yaml`" |
| "Make sure it's correct" | "Confirm the output matches: `[expected output]`" |

### Consistent Terminology

Pick one term for each concept and use it everywhere:
- Don't alternate between "server," "instance," "machine," and "host"
- Don't alternate between "restart," "reboot," and "cycle"
- Create a terminology section at the top if needed

## Procedure Maintenance

### When to Update

- After every execution where something was unclear or wrong
- When the system changes (new versions, new infrastructure)
- During quarterly review cycles

### Version Tracking

Include in every procedure:
- **Last verified:** Date someone successfully followed it
- **Last updated:** Date the content was modified
- **Author/maintainer:** Who to contact with questions

### Deprecation

When a procedure is no longer needed:
1. Mark it as deprecated in the title
2. Explain why and what replaced it
3. Keep it accessible for 90 days (someone might be mid-process)
4. Then archive

## Procedure Testing Framework

### Level 1: Desk Check
- Read through every step
- Check: Are commands correct? Are paths right? Are expected outputs accurate?
- Time: 15-30 minutes

### Level 2: Shadow Run
- One person follows the runbook step by step
- An expert watches and notes where they hesitate, get confused, or make errors
- Revise after every finding

### Level 3: Solo Run
- Someone who hasn't used this runbook follows it independently
- They document any questions or confusion
- If they complete it successfully without asking for help, the runbook passes

### Level 4: Stress Test
- Execute under realistic conditions (time pressure, incomplete information)
- This reveals whether the runbook works when it matters most
