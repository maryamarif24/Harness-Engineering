# Project 7: Harness Complete

## Objective
Demonstrate the "complete harness" pattern end-to-end, showing the morning-triage loop hardened across both Claude Code and OpenCode tooling, as described in Concept 9 of the Harness Engineering crash course.

## What This Project Covers
- **The morning-triage loop**: One full beat of the big loop, from prompt to completion
- **Hardened end-to-end**: Applying all five verbs (constrain, inform, verify, correct, escalate) plus observability
- **Both tools**: The pattern demonstrated in Claude Code and OpenCode
- **Part 5 of the crash course**: "A complete harness, twice"

## Setup
1. **Full harness configuration**: All five verbs plus observability active
2. **Loop structure**: Heartbeat, beats, and spine integration
3. **End-to-end tracing**: From prompt to completion with all verb interactions
4. **Both-tool comparison**: Claude Code vs OpenCode pattern parity

## Key Learnings
- The small loop (inner loop) lives inside one beat
- A beat is one full run of the big loop
- The spine (progress.md) survives between runs because the model forgets everything
- The same harness pattern works across different tooling (Claude Code vs OpenCode)
- "Agent = Model + Harness": the model is the same, the harness makes the difference
- Part 5 of the crash course teaches you to harden the loop end-to-end

## Demonstration

### The Morning-Triage Loop (One Beat)

```
┌─────────────────────────────────────────────────────────────────┐
│  9am: Beat starts ──────────────────────────────────────────────│
│  │                                                       │
│  │  1. Constrain: Permission rules active                   │
│  │     - Deny: rm -rf *, git push --force *                 │
│  │     - Ask: bash:*, edit                                  │
│  │     - Allow: Read, npm test *, git diff *                │
│  │                                                       │
│  │  2. Inform: Rules file + skills active                   │
│  │     - AGENTS.md conventions                              │
│  │     - daily-triage skill loaded when task matches        │
│  │     - Connectors (MCP servers) attached as needed        │
│  │                                                       │
│  │  3. Verify: Hooks + typed output active                  │
│  │     - before:bash validates commands                     │
│  │     - after:edit checks syntax                           │
│  │     - Output JSON schema: status, artifacts, checksum    │
│  │                                                       │
│  │  4. Correct: Recovery + ratchet ready                     │
│  │     - Checkpoints saved before risky ops                 │
│  │     - Failure classes tracked for ratchet habit          │
│  │                                                       │
│  │  5. Escalate: Human gate for risky actions               │
│  │     - git push --force * always escalates                │
│  │     - Trace recorded, human reviews if needed            │
│  │                                                       │
│  │  6. Observability: Trace + checkpoint + audit            │
│  │     - Full trace recorded                               │
│  │     - Checkpoint before git push                         │
│  │     - /doctor audit active                              │
│  │                                                       │
│  │  7. Beat completes ─────────────────────────────────────────│
│  └─────────────────────────────────────────────────────────────────┘
```

### Claude Code vs OpenCode Pattern Parity

| Surface | Claude Code | OpenCode | Parity |
|---------|-------------|----------|--------|
| **Constrain** | `settings.json` permissions | `opencode.json` permission | ✅ Same pattern, different file |
| **Inform** | `CLAUDE.md` + skills | `AGENTS.md` + skills | ✅ Same concepts, different files |
| **Verify** | Hooks + typed output | Hooks + typed output | ✅ Same mechanic |
| **Correct** | Recovery + ratchet | Recovery + ratchet | ✅ Same habit |
| **Escalate** | Human gate + logs | Human gate + traces | ✅ Same safety net |
| **Observability** | `/doctor` + traces | `/doctor` + traces | ✅ Same audit |

> **Key takeaway**: The harness shape is identical; only the file names differ.

### Hartened Loop in Both Tools

**Claude Code Example:**
```json
// settings.json
{
  "permissions": {
    "allow": ["Read", "Bash(npm test *)", "Bash(git diff *)"],
    "ask": ["Bash(*)", "edit"],
    "deny": ["Bash(rm -rf *)", "Bash(git push --force *)"]
  },
  "hooks": {
    "before:bash": "validate_command",
    "after:edit": "check_syntax"
  }
}
```

**OpenCode Example:**
```json
// opencode.json
{
  "permission": {
    "edit": "ask",
    "bash": {
      "*": "ask",
      "npm test*": "allow",
      "git diff*": "allow",
      "git push --force*": "deny"
    }
  }
}
```

**Same harness pattern, different file names.**

### The Spine (progress.md)

The spine survives between runs because the model forgets everything. It's a state file that tracks:
- What was accomplished in previous beats
- Checkpoint locations
- Ratchet fixes applied
- Trace summaries

**Spine Example (progress.md):**
```
# Project Triage Progress
## Beat: 2026-09-11-triage
### Completed: 
- Tests passed: 8/10
- Files modified: src/index.js
- Checkpoint: before-git-push

### Ratchet Fixes Applied:
1. Added deny rule: Bash(rm -rf *) 
2. Added hook: after:edit checks syntax

### Next Beat Triggers:
- 9am weekdays
- New issue tagged "triage"
- Manual: opencode run "run the daily-triage skill"
```

## How Project 7 Relates to Projects 1-6

```
Project-1 (Constrain):   Hard limits the agent cannot cross
Project-2 (Inform):      Provides capabilities and context  
Project-3 (Verify):      Proves work is correct before it counts
Project-4 (Correct):     Fixes systemic issues so mistakes don't recur
Project-4 (Escalate):    Provides safety net when system can't decide
Project-6 (Observability): Diagnoses, learns, and improves the system
Project-7 (Complete):    **All of the above, hardened end-to-end**
```

**Project 7 demonstrates that the complete harness pattern works across both major tooling platforms.**

## How This Fits the Complete Framework

```
All 5 verbs (Projects 1-5) + Observability (Project 6) + Complete harness (Project 7)
     ↓
Agent = Model + Harness fully demonstrated
     ↓
Can build, maintain, and evolve reliable agent harnesses
     ↓
Ready for Part 5 & 6 deep dive, or independent harness engineering
```

**The complete 7-project framework demonstrates that the harness is where engineering judgment lives, and that the same pattern works regardless of which tool you use.**