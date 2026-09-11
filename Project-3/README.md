# Project 3: Harness Verify

## Objective
Set up harness verify mechanisms (hooks, typed output, recovery, ratchet) to demonstrate the "verify" verb from the harness engineering crash course.

## What This Project Covers
- **Verify verb**: Prove the work before it counts
- **Hooks**: Code that runs automatically at set moments
- **Typed output**: Output in machine-checkable shape (JSON with named fields)
- **Recovery**: Re reading the system prompt to understand
<tool_call>
<function=bash>
<parameter=command>
ls -la /d/IT Course/Quarter 5/Har - when something goes wrong, recover the run
- **The ratchet**: Turning every mistake into a permanent harness fix
- **Four failure classes**: The agent didn't know, wasn't stopped, wasn't checked, or planned badly

## Setup
1. **Hooks configuration**: Define which hook events are active (before tool run, after edit, at end)
2. **Typed output schemas**: Define JSON schemas that work output must conform to
3. **Recovery procedures**: Define how failed runs recover to checkpoints
4. **Failure class tracking**: Categorize failures for the ratchet habit

## Key Learnings
- Verification catches bad steps early in a chain
- Typed output enables automated validation (code can validate, not just human review)
- Recovery resumes instead of restarting the whole run
- The ratchet habit: every mistake → permanent harness fix
- The four failure classes help identify which harness surface needs improvement
- "Trust but verify" - the harness verifies work the model produces

## Demonstration

### Typed Output Schema
```json
{
  "type": "object",
  "properties": {
    "status": {"type": "string", "enum": ["pass", "fail", "blocked"]},
    "test_results": {"type": "array", "items": {"type": "string"}},
    "execution_time": {"type": "number"},
    "checksum": {"type": "string"}
  },
  "required": ["status", "test_results"]
}
```

### Hook Events
- **before: tool** - Inspect before action runs
- **after: edit** - Validate after file modification  
- **at: end** - Summary and checkpoint save

### Failure Classes
| Class | Meaning | Harness Fix |
|-------|---------|-------------|
| 1 | Agent didn't know | Add to rules file / skills |
| 2 | Agent wasn't stopped | Add deny/ask rule |
| 3 | Agent wasn't checked | Add hook / typed output |
| 4 | Agent planned badly | Add to rules file / improve AX |

### The Ratchet Habit
Every mistake → permanent harness fix:
1. Identify failure class
2. Fix the right harness surface
3. Document in rules file / skills
4. Never same mistake twice
```