# Project 4: Harness Correct

## Objective
Set up harness correct mechanisms (recovery, ratchet, failure classes) to demonstrate the "correct" verb from the harness engineering crash course.

## What This Project Covers
- **Correct verb**: When something goes wrong, recover the run, then change the harness so the mistake never repeats
- **The ratchet habit**: Turning every mistake into a permanent harness fix
- **Recovery procedures**: How failed runs resume from checkpoints
- **Four failure classes**: The agent didn't know, wasn't stopped, wasn't checked, or planned badly
- **Typed output recovery**: Using schemas to validate recovery actions

## Setup
1. **Recovery configuration**: Define how failed runs recover to checkpoints
2. **Failure class tracking**: Categorize failures for the ratchet habit
3. **Ratched documentation**: Document every mistake and its permanent fix
4. **Recovery audit**: Use `/doctor` to audit recovery setup

## Key Learnings
- Recovery resumes instead of restarting the whole run
- The ratchet habit: every mistake → permanent harness fix
- The four failure classes help identify which harness surface needs improvement
- "A mistake that is not fixed is a mistake that will repeat"
- Recovery is cheaper than restart: checkpoints save time and context
- After recovery, the harness must be changed so the mistake never happens again

## Demonstration

### The Ratchet Habit Workflow
1. **Identify** the mistake and its failure class
2. **Recover** the run from the last checkpoint
3. **Fix** the correct harness surface (deny rule, hook, typed schema)
4. **Document** the fix in the rules file or skills
5. **Verify** the same mistake cannot recur

### Four Failure Classes
| Class | Meaning | Correct Surface |
|-------|---------|-----------------|
| 1 | Agent didn't know | Add to rules file / add skill |
| 2 | Agent wasn't stopped | Add deny/ask rule (Constrain) |
| 3 | Agent wasn't checked | Add hook / typed output (Verify) |
| 4 | Agent planned badly | Improve prompt / improve AX (Inform) |

### Recovery Example
```
Before: Agent deletes test folder → "Done! All tests pass."
After harness fix: 
  1. Deny rule: Bash(rm -rf *) blocks deletion
  2. Hook: after:bash validates command intent
  3. Recovery: Run resumes from last checkpoint
  4. Documentation: Added to AGENTS.md "never delete test folders"
  5. Ratchet: Same mistake cannot recur
```