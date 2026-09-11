# Project 4: Harness Correct

This project demonstrates the user (Correct verb) from the Harness Engineering crash course.

## Correct Verb Structure

The correct verb answers: **"When something goes wrong, recover the run, then change the harness so the mistake never repeats."**

Unlike constrain (which says "no"), inform (which says "here's what you need"), and verify (which says "show me you did it right"), correct says "fix the system so this doesn't happen again." All four verbs work together mechanically.

## The Ratchet Habit

> **"A mistake that is not fixed is a mistake that will repeat."**

The ratchet is the habit of turning every mistake into a permanent harness fix, so it can never happen again.

### Ratchet Workflow
1. **Capture** the mistake (what happened, what failed, failure class)
2. **Recover** the run from the last checkpoint
3. **Fix** the correct harness surface
4. **Document** the fix permanently (rules file, skills, AGENTS.md)
5. **Verify** the same mistake cannot recur

## Three Correct Surfaces

### 1. Recovery Procedures

Answers: ***when something goes wrong, how do we resume?***

Recovery procedures that let a failed run resume from a checkpoint instead of restarting completely.

**Example Recovery Configuration:**
```json
{
  "recovery": {
    "checkpoint_before": "major_step",
    "resume_from_last_good": true,
    "log_recovery": true
  }
}
```

### 2. Failure Class Tracking

Answers: ***what kind of thing went wrong?***

The four failure classes help identify which harness surface needs improvement:

| Class | Meaning | Correct Surface |
|-------|---------|-----------------|
| 1 | Agent didn't know | Add to rules file / add skill |
| 2 | Agent wasn't stopped | Add deny/ask rule (Constrain) |
| 3 | Agent wasn't checked | Add hook / typed output (Verify) |
| 4 | Agent planned badly | Improve prompt / improve AX (Inform) |

### 3. The Ratchet Documentation

Answers: ***permanent record of every fix***

Every mistake must be documented so it never happens again. Documentation lives in:
- `AGENTS.md` - project conventions and lessons
- `SKILL.md` - saved instructions for specific jobs
- Commit messages - "fix: never X again"

**Example Documentation:**
```
# Ratchet: Never delete test folders

**Mistake**: Agent ran `rm -rf test/` and deleted the test suite
**Failure Class**: 2 (agent wasn't stopped)
**Fix**: Added deny rule `Bash(rm -rf *)` to opencode.json
**Documented**: Added to AGENTS.md "Never delete test folders without confirmation"
**Verified**: Same mistake cannot recur (deny rule enforced)
```

## How Correct Complements Constrain + Inform + Verify

```
Constrain: "No, you cannot do that"
Inform:  "Here's what you need to do it"
Verify:  "Show me you did it right"
Correct: "Fix the system so this doesn't happen again"
```

All four verbs work together to make the agent reliable:
- **Constrain** limits what can go wrong
- **Inform** ensures the agent has what it needs
- **Verify** proves the work is correct before it counts
- **Correct** fixes the system:setup;