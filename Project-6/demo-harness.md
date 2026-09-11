# Project 6: Harness Observability

This project demonstrates the **observability** and **staying the engineer** concepts from Part 6 of the Harness Engineering crash course.

## Observability Verb Structure

The observability verb answers: **"Let me see what happened, so I can understand and improve."**

Unlike the other five verbs that act as controls or enablers, observability is about **diagnosis and improvement** - it's how the engineer learns from the agent's behavior and makes the whole system better over time.

## Three Observability Surfaces

### 1. Traces

Answers: ***what happened step-by-step?***

The recorded step-by-step story of one run: every tool call, every result, every decision point.

**Trace Components:**
- **Run metadata**: ID, start/end time, overall status
- **Tool calls**: Every tool requested, with timestamp and parameters
- **Tool results**: Every return value, success/failure, execution time
- **Decision points**: Every ask/deny/allow outcome with context
- **Checkpoint markers**: Where checkpoints were saved/resumed

**Trace Format Example:**
```json
{
  "run_id": "2026-09-11-14-30-00",
  "start_time": "2026-09-11T14:30:00Z",
  "end_time": "2026-09-11T14:35:22Z",
  "overall_status": "completed",
  "actions": [
    {
      "timestamp": "2026-09-11T14:30:10Z",
      "phase": "tool_call",
      "tool": "Bash",
      "parameters": {"command": "git diff"},
      "status": "allowed",
      "result": "Showing changes between HEAD and working tree"
    },
    {
      "timestamp": "2026-09-11T14:30:15Z",
      "phase": "tool_result",
      "tool": "Bash",
      "parameters": {"command": "git diff"},
      "status": "success",
      "output": "diff --git a/src/index.js b/src/index.js\n..."
    },
    {
      "timestamp": "2026-09-11T14:30:20Z",
      "phase": "decision",
      "tool": "ask",
      "parameters": {"action": "git push --force origin main"},
      "status": "human_rejected",
      "context": "Force push to main - data loss risk"
    }
  ],
  "checkpoints": ["before-git-push-2026-09-11"],
  "failure_class": null,
  "ratchet_fix": null
}
```

### 2. Checkpoints

Answers: ***where can we resume from?***

Saved good states that a run can roll back to or resume from. Checkpoints are the recovery mechanism that makes observability practical - without them, you'd have to start from scratch every time something goes wrong.

**Checkpoint Best Practices:**
- **Save before risky operations** (git push, database writes, file deletions)
- **Label checkpoints descriptively** ("before-git-push-2026-09-11", "after-test-suite-8")
- **Store what's essential** (project state, test results, branch head, not entire disk)
- **Retain for appropriate time** (24 hours for development, 7 days for production)
- **Automatically expire** old checkpoints to manage storage

**Checkpoint Example:**
```
# Label: "before-git-push-2026-09-11-14-30"
# Saved: 2026-09-11 at 14:30:00
# Contains:
#   - Git HEAD commit hash: a1b2c3d
#   - Test results: 8 passing, 2 failing
#   - Branch: feature/triage
#   - Modified files: src/index.js, tests/api.test.js
#   - Current working directory: /workspace/project

# Recovery procedure:
# 1. Run: opencode recover --checkpoint before-git-push-2026-09-11-14-30
# 2. Restore: git checkout a1b2c3d
# 3. Resume: Continue from saved state
# 4. Log: Record in AGENTS.md as ratchet fix
```

### 3. Observability Audit

Answers: ***how healthy is my harness?***

Using `/doctor`-style checkups to audit your whole harness setup and identify what's working and what needs improvement.

**Audit Checklist:**
- [ ] All deny rules are still needed (remove unused ones)
- [ ] All skills are still loaded (remove unused ones)
- [ ] Checkpoints are being saved before risky ops
- [ ] Traces are being recorded with sufficient detail
- [ ] Failure classes are being tracked and categorized
- [ ] The ratchet habit is being maintained (every fix documented)
- [ ] Harness coupling is understood (changing one surface affects others)
- [ ] Control trade-off is evaluated (not too many, not too few rules)
- [ ] Human gate is working (escalations are reviewed, not ignored)
- [ ] Observability data is actually being reviewed (not just collected)

**/doctor-Style Output Example:**
```
=== Harness Observatory Audit ===

✅ Deny rules: 5 active, 2 unused (candidates for removal)
✅ Skills: 3 active, all triggered in last 30 days
✅ Checkpoints: Saved before all risky ops
✅ Traces: Recorded with full detail
✅ Failure classes: All categories covered
⚠️  Ratchet: 3 fixes documented, 1 missing documentation
⚠️  Coupling: Changing Constrain surface affects Verify hooks
⚠️  Trade-off: 12 rules current, consider removing 2 unused
✅ Escalation: Human gate reviewed monthly

=== Recommendations ===
1. Remove 2 unused deny rules (reduces coupling)
2. Document 1 missing ratchet fix
3. Evaluate if 2 skills can be consolidated
3. Review human gate escalations quarterly

=== Overall Health: 75/100 ⬆️ +5% from last month
```

### The Law of Diminishing Returns

**Every new harness rule:**
1. ✅ Reduces a class of actual failures
2. ❌ Adds maintenance overhead
3. ❌ Increases coupling with other surfaces
4. ❌ Increases cognitive load for the engineer
5. ❌ Eventually: net negative effect on reliability

**Rule of thumb:**
- Add rules to address **actual** failures (not potential ones)
- Remove rules that haven't been triggered in **90 days**
- **Audit harness monthly** with `/doctor`
- **Prioritize fixing root causes** over adding surface-level rules
- **Goal**: Minimal harness, maximal reliability - **not** maximum rules, maximum reliability

**The best harness rule is the one you don't need.**

## How Observability Complements the Other Five Verbs

```
Constrain:  "No, you cannot do that"          (hard limits)
Inform:     "Here's what you need to do it"   (capabilities)
Verify:     "Show me you did it right"        (proof)
Correct:    "Fix the system so it doesn't recur" (permanent fixes)
Escalate:   "When in doubt, involve a human"   (safety net)
Observability: "Let me see what happened"    (diagnostics and improvement)
```

**All six concepts work together to make the agent reliable and the engineer effective:**

1. **Constrain** sets hard limits on what the agent can do
2. **Inform** provides the agent with what it needs (rules, skills, tools)
3. **Verify** proves the work is correct before it counts
4. **Correct** fixes systemic issues so mistakes don't repeat
5. **Escalate** provides a safety net when the system can't decide
6. **Observability** enables the engineer to diagnose, learn, and improve

**Without observability**, the other five verbs operate in darkness - you know what you constrained, what you informed, what was verified, what was corrected, and when things escalated, but you have no data on whether it's actually working or how to make it better.

**With observability**, you can:
- See which rules are actually triggered vs. unused
- Identify which failure classes are most common
- Track the ratchet habit effectiveness
- Evaluate the control trade-off quantitatively
- Make evidence-based decisions about harness evolution
- Demonstrate ROI on harness investments to stakeholders

## Key Takeaways

1. **Observability over assumption**: You must see what the agent did and why
2. **Traces enable debugging**: Step-by-step story of one run
3. **Checkpoints save time**: Resume from last good state, don't restart
4. **Control trade-off**: More rules = less surprise but more maintenance
5. **Harness coupling**: Changing one surface affects others - think systemically
6. **Law of diminishing returns**: Adding more harness rules eventually decreases reliability
7. **Know which ring your bug lives in**: Inner vs outer harness determines where the fix lives
8. **"The best harness rule is the one you don't need"**: Minimal harness, maximal reliability
9. **Observation enables improvement**: You can't improve what you can't measure
10. **The complete harness**: All 6 surfaces work together for reliability AND engineer effectiveness

## How Project 6 Relates to Projects 1-5

```
Project-1 (Constrain):  Sets hard limits the agent cannot cross
Project-2 (Inform):     Provides capabilities and context
Project-3 (Verify):     Proves work is correct before it counts
Project-4 (Correct):    Fixes systemic issues so mistakes don't recur
Project-5 (Escalate):   Provides safety net when system can't decide
Project-6 (Observability): Diagnoses, learns, and improves the whole system
```

**The first 5 projects make the agent reliable; Project 6 makes the engineer effective.**

Together, all 6 projects demonstrate that **Agent = Model + Harness**, and that the harness is where engineering judgment lives - not in the model itself.

---

**This is Project 6 of 6 in the Harness Engineering crash course.**

**After this project, you have the complete framework:**
- All 5 verbs (constrain, inform, verify, correct, escalate)
- All 6 surfaces (including observability)
- The complete understanding of how to build, maintain, and evolve reliable agent harnesses
- The knowledge of when to add rules and when to remove them
- The habits (the ratchet, the audit, the control trade-off) that distinguish harness engineers from SDK users