# Project 5: Harness Escalate

This project demonstrates the **escalate** verb from the Harness Engineering crash course.

## Escalate Verb Structure

The escalate verb answers: **"When the harness cannot decide, send it to a person, visibly."**

Unlike constrain (which says "no"), inform (which says "here's what you need"), verify (which says "show me you did it right"), and correct (which says "fix the system"), escalate says "involve a human." All five verbs work together mechanically.

## The Human Gate

> **"Risky or failed work goes to a person, never straight to main."**

The human gate is the principle that certain actions should never be automated, no matter how well-constrained, informed, verified, or corrected the system is.

### Human Gate Workflow

1. **Risky action identified** (e.g., `git push --force`, `rm -rf *`, database deletion)
2. **Harness prompts for human confirmation** (ask verb in action)
3. **Human reviews** the action context and consequences
4. **Human approves or denies** the action
5. **Action proceeds** only with human yes, otherwise halted

### Actions That Always Escalate

| Action | Reason | Escalation Required |
|--------|--------|---------------------|
| `git push --force *` to main | History rewriting, data loss risk | ✅ Always |
| `rm -rf *` on production | Complete directory deletion | ✅ Always |
| Database DELETE without WHERE | Mass data deletion | ✅ Always |
| Any action with data loss > 1 hour | Insufficient undo history | ✅ Always |
| Production secrets access | Credential exposure risk | ✅ Always |
| Force-push to shared branches | Coordination risk with other developers | ✅ Always |

### Trace Recording

Every run produces a trace automatically:
- **Every tool call** (what the agent requested)
- **Every result** (what the tool returned)
- **Every decision point** (ask/deny/allow outcomes)
- **Execution timeline** (what happened and when)

**Trace Format Example:**
```json
{
  "run_id": "2026-09-11-14-30-00",
  "start_time": "2026-09-11T14:30:00Z",
  "actions": [
    {
      "timestamp": "2026-09-11T14:30:10Z",
      "tool": "Bash",
      "command": "git push --force origin main",
      "result": "denied by harness - human gate",
      "escalated": true
    },
    {
      "timestamp": "2026-09-11T14:30:15Z",
      "tool": "Bash",
      "command": "git push origin main",
      "result": "approved by human",
      "escalated": false
    }
  ],
  "checkpoints_saved": 3,
  "overall_status": "completed"
}
```

### Checkpoint System

- **Save before risky operations** (git push, database writes, file deletions)
- **Resume from last checkpoint** if something goes wrong
- **Rollback to known good state** instead of starting fresh
- **Log the checkpoint** for the ratchet habit

**Checkpoint Example:**
```
# Save checkpoint before git push
# Label: "before-git-push-2026-09-11"
# Contains: project state, test results, branch head

# If push fails:
# Resume from "before-git-push-2026-09-11"
# Restore project state
# Continue with human-approved action
```

## How Escalate Complements the Other Four Verbs

```
Constrain:  "No, you cannot do that"          (hard limit - never reaches human)
Inform:     "Here's what you need to do it"   (enable capability when safe)
Verify:     "Show me you did it right"        (prove correctness after)
Correct:    "Fix the system so it doesn't recur" (permanent fix after human review)
Escalate:   "When in doubt, involve a human"   (safety net - when unsure)
```

**The escalate verb is the safety net that ensures no single verb bears the full burden of risk.**

It acknowledges that some decisions simply should not be automated, no matter how well-constrained, informed, verified, or corrected the system is.

### The 5-Verb Stack in Practice

```
Project-1 (Constrain):  Agent tries rm -rf * → DENIED by harness
Project-2 (Inform):     Agent needs to fetch data → TOOL PROVIDED with AX
Project-3 (Verify):      Agent runs tests → OUTPUT SCHEMA validated
Project-4 (Correct):     Agent makes mistake → HARNESS FIX applied
Project-5 (Escalate):    Agent encounters unknown → HUMAN GATE triggered
```

## Key Takeaways

1. **Human gate over automation**: Risky work → human, not automated
2. **Observability is required**: You must see what the agent did and why
3. **Traces enable debugging**: Step-by-step story of one run
4. **Checkpoints save time**: Resume from last good state, don't restart
5. **Escalate when uncertain**: Better to involve human than automate risk
6. **The complete harness**: All 5 verbs work together for reliability
7. **The business value**: "Humans steer. Agents execute." (OpenAI discipline tagline)

The escalate verb ensures that the harness remains under human control, even as the model becomes more capable. It's the principle that some decisions are simply too important to automate.