# Project 6: Harness Observability

## Objective
Set up harness observability mechanisms (logs, traces, checkpoints, control trade-off) to demonstrate the "staying the engineer" concepts from Part 6 of the harness engineering crash course.

## What This Project Covers
- **Observability**: Being able to see what the agent did and why, after the fact
- **Traces**: The recorded step-by-step story of one run
- **Checkpoints**: Saved good states that a run can roll back to or resume from
- **Control trade-off**: Balancing automation with human oversight
- **Harness coupling**: How harness surfaces interact and depend on each other
- **When to stop adding rules**: The law of diminishing returns on harness rules

## Setup
1. **Trace configuration**: Define what step-by-step data gets recorded during each run
2. **Checkpoint system**: Save state before major operations for recovery
3. **Observability audit**: Use `/doctor` to audit your observability setup
4. **Control trade-off assessment**: Evaluate where automation ends and human oversight begins

## Key Learnings
- Observability is being able to see what the agent did and why, after the fact
- A trace is the recorded step-by-step story of one run: every tool call, every result
- A checkpoint is a saved good state that a run can roll back to or resume from
- The control trade-off: more rules = less surprise but more maintenance
- Harness coupling: surfaces interact - changing one surface affects others
- **The law of diminishing returns**: Adding more harness rules eventually decreases reliability
- "Know which ring your bug lives in before you try to fix it" - inner vs outer harness

## Demonstration

### Trace Recording
Every run produces trace data automatically:
- **Every tool call** (what the agent requested)
- **Every result** (what the tool returned)
- **Every decision point** (ask/deny/allow outcomes)
- **Execution timeline** (what happened and when)

**Trace Format Example:**
```json
{
  "run_id": "2026-09-11-14-30-00",
  "start_time": "2026-09-11T14:30:00Z",
  "end_time": "2026-09-11T14:35:22Z",
  "actions": [
    {
      "timestamp": "2026-09-11T14:30:10Z",
      "tool": "Bash",
      "command": "git diff",
      "status": "allowed",
      "result": "Showing changes between HEAD and working tree"
    },
    {
      "timestamp": "2026-09-11T14:30:15Z",
      "tool": "Bash",
      "command": "npm test",
      "status": "allowed",
      "result": "8 passing, 2 failing"
    }
  ],
  "checkpoints_saved": 2,
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

### Control Trade-Off Assessment

| More Rules | Less Surprise | More Maintenance | Net Effect |
|-----------|---------------|------------------|------------|
| +1 rule   | -5% failures    | +10% maintenance time | ❌ Negative |
| +3 rules  | -15% failures   | +25% maintenance time | ❌ Negative |
| +0 rules  | Baseline        | Baseline           | ✅ Optimal |
| -2 rules  | +8% failures    | -15% maintenance time | ⚠️ Risky |

### Harness Coupling

Changing one harness surface affects others:
- **Changing a deny rule** (Constrain) → may affect what Verify can check
- **Adding a skill** (Inform) → may reduce need for some hooks (Verify)
- **Fixing a recovery procedure** (Correct) → may change how Escalate triggers
- **Changing the human gate** (Escalate) → affects how Constrain rules are written

**Example Coupling:**
```
Constrain: Add deny rule for rm -rf *
  ↓
Verify:  Hook after:bash now validates rm commands
  ↓
Correct: Ratchet documents "never rm -rf *" in AGENTS.md
  ↓
Escalate: Human gate triggered if someone manually overrides
```

## Key Takeaways

1. **Observability over assumption**: You must see what the agent did and why
2. **Traces enable debugging**: Step-by-step story of one run
3. **Checkpoints save time**: Resume from last good state, don't restart
4. **Control trade-off**: More rules = less surprise but more maintenance
5. **Harness coupling**: Changing one surface affects others - think systemically
6. **Law of diminishing returns**: Adding more harness rules eventually decreases reliability
7. **Know which ring your bug lives in**: Inner vs outer harness determines where the fix lives
8. **"The best harness rule is the one you don't need"**: Minimal harness, maximal reliability

## How Observability Complements the Other Four Verbs

```
Constrain:  "No, you cannot do that"          (hard limits)
Inform:     "Here's what you need to do it"   (capabilities)
Verify:     "Show me you did it right"        (proof)
Correct:    "Fix the system so it doesn't recur" (permanent fixes)
Escalate:   "When in doubt, involve a human"   (safety net)
Observability: "Let me see what happened"    (diagnostics)
```

All six concepts work together to make the agent reliable and the engineer effective:
- **Constrain** sets hard limits
- **Inform** provides capabilities
- **Verify** proves correctness
- **Correct** fixes systemic issues
- **Escalate** provides safety net
- **Observability** enables diagnosis and improvement

## When to Stop Adding Rules

**The law of diminishing returns**: Every new harness rule:
1. Reduces a class of failures
2. Adds maintenance overhead
3. Increases coupling with other surfaces
4. Eventually: net negative effect on reliability

**Rule of thumb**: 
- Add rules to address actual failures (not potential ones)
- Remove rules that haven't been triggered in 90 days
- Audit harness monthly with `/doctor`
- Prioritize fixing root causes over adding surface-level rules

**The goal**: Minimal harness, maximal reliability - not maximum rules, maximum reliability.