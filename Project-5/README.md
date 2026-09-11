# Project 5: Harness Escalate

## Objective
Set up harness escalate mechanisms (human gate, logs, visible failure) to demonstrate the "escalate" verb from the harness engineering crash course.

## What This Project Covers
- **Escalate verb**: When the harness cannot decide, send it to a person, visibly
- **Human gate**: Risky or failed work goes to a person, never straight to main
- **Logs and traces**: Being able to see what the agent did and why, after the fact
- **Checkpoint system**: Saved good states that a run can roll back to or resume from
- **Trace recording**: The recorded step-by-step story of one run

## Setup
1. **Human gate configuration**: Define which actions require human approval before proceeding
2. **Logs and trace configuration**: Define what gets recorded during each run
3. **Checkpoint system**: Save state before risky operations so runs can resume
4. **Failure escalation**: Define when and how work gets escalated to a human

## Key Learnings
- The human gate: risky or failed work goes to a person, never straight to main
- Observability: Being able to see what the agent did and why, after the fact
- Trace: The recorded step-by-step story of one run (every tool call, every result)
- Checkpoint: A saved good state that a run can roll back to or resume from
- "Agent = Model + Harness": The harness determines what happens when the model fails
- "When in doubt, escalate": It is always better to involve a human than to automate a risky decision

## Demonstration

### Human Gate Workflow
1. **Risky action identified** (e.g., `git push --force`, `rm -rf *`, database deletion)
2. **Harness prompts for human confirmation** (ask verb in action)
3. **Human reviews** the action context and consequences
4. **Human approves or denies** the action
5. **Action proceeds** only with human yes, otherwise halted

### Trace Recording
Every run produces a trace automatically:
- **Every tool call** (what the agent requested)
- **Every result** (what the tool returned)
- **Every decision point** (ask/deny/allow outcomes)
- **Execution timeline** (what happened and when)

### Checkpoint System
- **Save before risky operations** (git push, database writes, file deletions)
- **Resume from last checkpoint** if something goes wrong
- **Rollback to known good state** instead of starting fresh
- **Log the checkpoint** for the ratchet habit

### Escalation Triggers
Actions that always escalate to human:
- `git push --force *` to main branch
- `rm -rf *` on production directories
- Database DELETE operations without WHERE clause
- Any action with potential data loss > 1 hour
- Any action affecting production secrets or credentials

## How Escalate Complements Constrain + Inform + Verify + Correct

```
Constrain:  "No, you cannot do that"          (hard limit)
Inform:     "Here's what you need to do it"   (enable capability)
Verify:     "Show me you did it right"        (prove correctness)
Correct:    "Fix the system so it doesn't recur" (permanent fix)
Escalate:   "When in doubt, involve a human"   (safety net)
```

All five verbs work together to make the agent reliable:
- **Constrain** sets hard limits (can't do)
- **Inform** provides capabilities (can do)
- **Verify** proves work is correct (did it right)
- **Correct** fixes systemic issues (won't recur)
- **Escalate** provides safety net (human decides when unsure)

The escalate verb is the safety net that ensures no single verb bears the full burden of risk. It acknowledges that some decisions simply should not be automated, no matter how well-constrained, informed, verified, or corrected the system is.

## Key Takeaways

1. **Human gate over automation**: Risky work → human, not automated
2. **Observability is required**: You must see what the agent did and why
3. **Traces enable debugging**: Step-by-step story of one run
4. **Checkpoints save time**: Resume from last good state, don't restart
5. **Escalate when uncertain**: Better to involve human than automate risk
6. **The complete harness**: All 5 verbs work together for reliability