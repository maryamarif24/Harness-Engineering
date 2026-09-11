# Project 7: Harness Complete

This project demonstrates the **complete harness** pattern end-to-end, as described in Concept 9 of the Harness Engineering crash course.

## Complete Harness Pattern

The complete harness demonstrates all five verbs (constrain, inform, verify, correct, escalate) plus observability (the 6th surface) working together in one end-to-end loop.

### The Five Verbs, One Loop

```
Constrain:  "No, you cannot do that"          (hard limits - Project-1)
Inform:     "Here's what you need to do it"   (capabilities - Project-2)
Verify:     "Show me you did it right"        (proof - Project-3)
Correct:    "Fix the system so it doesn't recur" (permanent fixes - Project-4)
Escalate:   "When in doubt, involve a human"   (safety net - Project-5)
Observability: "Let me see what happened"    (diagnostics - Project-6)
Complete:   "The whole system working together" (Project-7)
```

### End-to-End Loop Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│  ONE BEAT (MORNING TRIAGE LOOP)                               │
├─────────────────────────────────────────────────────────────────┤
│  ☐ 1. CONSTRAIN - Permission rules mechanically enforced       │
│     Deny: rm -rf *, git push --force *                       │
│     Ask: bash:*, edit                                        │
     Allow: Read, npm test *, git diff *                        │
├─────────────────────────────────────────────────────────────────┤
│  ☐ 2. INFORM - Rules file + skills provide context            │
│     AGENTS.md: project conventions                           │
     daily-triage skill: loaded when task matches               │
     Connectors: MCP servers attached as needed                │
├─────────────────────────────────────────────────────────────────┤
│  ☐ 3. VERIFY - Hooks + typed output prove correctness         │
│     before:bash validates commands                           │
     after:edit checks syntax                                  │
     Output JSON schema: status, artifacts, checksum           │
├─────────────────────────────────────────────────────────────────┤
│  ☐ 4. CORRECT - Recovery + ratchet fix permanent issues       │
     Checkpoints saved before risky ops                       │
     Failure classes tracked                                  │
     "Never the same mistake twice" documented                 │
├─────────────────────────────────────────────────────────────────┤
│  ☐ 5. ESCALATE - Human gate for risky actions                │
     git push --force * always escalates                       │
     Trace recorded, human reviews if needed                   │
├─────────────────────────────────────────────────────────────────┤
│  ☐ 6. OBSERVABILITY - Trace + checkpoint + audit             │
     Full trace recorded                                       │
     Checkpoint before git push                                │
     /doctor audit active                                      │
├─────────────────────────────────────────────────────────────────┤
│  ✅ BEAT COMPLETE - All verbs active, loop finishes           │
└─────────────────────────────────────────────────────────────────┘
```

### Claude Code vs OpenCode Parity

| Surface | Claude Code (`settings.json`) | OpenCode (`opencode.json`) | Parity |
|---------|------------------------------|---------------------------|--------|
| **Constrain** | `{"permissions": {"allow": [...], "ask": [...], "deny": [...]}}` | `{"permission": {"edit": "ask", "bash": {"*": "ask", ...}}}` | ✅ Same pattern |
| **Inform** | `CLAUDE.md` + skills | `AGENTS.md` + skills | ✅ Same concepts |
| **Verify** | Hooks + typed output | Hooks + typed output | ✅ Same mechanic |
| **Correct** | Recovery + ratchet | Recovery + ratchet | ✅ Same habit |
| **Escalate** | Human gate + logs | Human gate + traces | ✅ Same safety net |
| **Observability** | `/doctor` + traces | `/doctor` + traces | ✅ Same audit |

> **Key takeaway**: The harness shape is identical across tools; only file names differ.

### The Spine (progress.md)

The spine survives between runs because the model forgets everything. It's a state file that tracks what was accomplished, checkpoints, and ratchet fixes.

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
Project-5 (Escalate):    Provides safety net when system can't decide
Project-6 (Observability): Diagnoses, learns, and improves the system
Project-7 (Complete):    **All of the above, hardened end-to-end**
```

**Project 7 demonstrates that the complete harness pattern works across both major tooling platforms (Claude Code and OpenCode), with the same five verbs plus observability all working together.**

## The Complete 7-Project Framework

| Project | Focus | Key Outcome |
|---------|-------|-------------|
| **Project-1** | Constrain | Mechanically enforced permission rules |
| **Project-2** | Inform | Rules file, skills, connectors, AX design |
| **Project-3** | Verify | Hooks, typed output, failure classes, ratchet |
| **Project-4** | Correct | Recovery, permanent fixes via ratchet habit |
| **Project-5** | Escalate | Human gate, traces, checkpoints, safety net |
| **Project-6** | Observability | Traces, checkpoints, control trade-off, diminishing returns |
| **Project-7** | Complete | **All of the above, hardened end-to-end** |

**Total**: 7 projects, 28 files (4 per project), demonstrating the complete **Harness Engineering crash course** framework where **Agent = Model + Harness**.

**Each project is**:
- ✅ Self-contained directory with its own `opencode.json`
- ✅ Tested and verified with OpenCode CLI
- ✅ Pushed to GitHub as separate directories
- ✅ Demonstrates one or more harness engineering concepts
- ✅ Works independently or as part of the complete 7-project framework

**The framework answers**: How to build agent harnesses that turn model intelligence into reliable, production-ready automation - and how to maintain them over time.
```