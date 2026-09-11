# Project 3: Harness Verify

This project demonstrates the **verify** verb from the Harness Engineering crash course.

## Verify Verb Structure

The verify verb answers: **"Prove the work before it counts."**

Unlike constrain (which says "no") and inform (which says "here's what you need"), verify says "show me you did it right." All three are mechanically enforced by the harness.

## Three Verify Surfaces

### 1. Hooks

Answers: ***when should the harness intervene?***

Code that the harness runs automatically at set moments:
- **before: tool** - Inspect before action runs
- **after: edit** - Validate after file modification
- **at: end** - Summary and checkpoint save

**Example Hook Configuration:**
```json
{
  "hooks": {
    "before:bash": "validate_command",
    "after:edit": "check_syntax",
    "at:end": "save_checkpoint"
  }
}
```

### 2. Typed Output

Answers: ***what shape must the work be in?***

Output in a fixed, machine-checkable shape (JSON with named fields), so code can validate it.

**Example Typed Output Schema:**
```json
{
  "type": "object",
  "properties": {
    "status": {"type": "string", "enum": ["pass", "fail", "blocked"]},
    "artifacts": {"type": "array", "items": {"type": "string"}},
    "execution_time": {"type": "number"},
    "checksum": {"type": "string"}
  },
  "required": ["status", "artifacts"]
}
```

> **Key Principle**: Typed output enables automated validation - code can validate, not just human review.

### 3. Recovery

Answers: ***when something goes wrong, how do we resume?***

Recovery procedures that let a failed run resume from a checkpoint instead of restarting completely.

**Example Recovery Workflow:**
1. Save checkpoint before each major step
2. On failure, identify the last good checkpoint
3. Resume from checkpoint, not restart
4. Log the recovery for the ratchet

## Key Takeaways

1. **Verification over assumption**: Verify catches bad steps early
2. **Typed output = automated validation**: Code validates, not just humans
3. **Recovery = resume, don't restart**: Checkpoints save time
4. **The ratchet habit**: Every mistake → permanent harness fix
5. **Four failure classes**: Identify which harness surface needs improvement
6. **"Trust but verify"**: The harness verifies work the model produces

## Demonstration

### Hooks in Action
When the agent runs a bash command, the `before:bash` hook can validate the command before execution. If the command matches a deny pattern, it's blocked automatically.

### Typed Output Example
After running tests, the agent outputs:
```json
{
  "status": "pass",
  "artifacts": ["test-report.xml"],
  "execution_time": 2.3,
  "checksum": "a1b2c3d4"
}
```

Code (not prompts) validates this JSON schema. If `status` is not "pass" or fields are missing, the harness flags it automatically.

### Failure Class Identification
When a test suite fails:
- **Class 1**: Add the test to rules file / skills
- **Class 2**: Add a deny rule for the failing command
- **Class 3**: Add a hook to validate before running
- **Class 4**: Improve the agent's planning AX

### The Ratchet Habit
1. **Identify** the failure class
2. **Fix** the right harness surface (deny rule, hook, typed schema)
3. **Document** in rules file or skills
4. **Never** make the same mistake twice

## How Verify Complements Constrain + Inform

```
Constrain: "No, you cannot do that"
Inform:  "Here's what you need to do it"
Verify:  "Show me you did it right"
```

All three verbs work together to make the agent reliable:
- **Constrain** limits what can go wrong
- **Inform** ensures the agent has what it needs
- **Verify** proves the work is correct before it counts

Together they form the complete harness that turns a bare model into a trustworthy agent.
```