# Harness Engineering Crash Course

Complete 8-project practice framework demonstrating all harness engineering concepts from the [AI Agent Factory](https://agentfactory.panaversity.org/docs/harness-engineering-crash-course) course.

## Repository Overview

This repository contains **8 practice projects** (32 files total) that build the complete harness engineering framework, one concept at a time. Each project is self-contained and demonstrates one or more of the five harness engineering verbs (plus observability), progressing from easy to hard.

## The Framework: Agent = Model + Harness

The core premise: **The model supplies intelligence. The harness turns that intelligence into something reliable.**

This repository demonstrates the **harness** — everything around the model that decides what it may do, what it knows, how its work is proven, and what happens when it goes wrong.

## Project Structure

| Project | Verb/Concept | Key Principle | Files |
|---------|-------------|---------------|-------|
| **Project-1** | **Constrain** | Deny > Ask > Allow priority, guardrails in harness not prompts | 4 files |
| **Project-2** | **Inform** | Rules file, skills, connectors, AX "show don't tell" | 4 files |
| **Project-3** | **Verify** | Hooks, typed output, failure classes, ratchet habit | 4 files |
| **Project-4** | **Correct** | Recovery, ratchet habit, permanent fixes | 4 files |
| **Project-5** | **Escalate** | Human gate, traces, checkpoints, safety net | 4 files |
| **Project-6** | **Observability** | Traces, checkpoints, control trade-off, diminishing returns | 4 files |
| **Project-7** | **Complete** | All 5 verbs + observability, end-to-end | 4 files |
| **Project-8** | **Mastery** | Capstone integration, self-assessment, next steps | 4 files |

**Total**: 32 files (8 projects × 4 files each)

## The 8 Practice Projects (Easy to Hard)

The [Harness Engineering crash course](https://agentfactory.panaversity.org/docs/harness-engineering-crash-course) includes eight practice projects, easy to hard:

1. **Project-1**: Basic Constraints → Setup permission rules (allow/ask/deny), mechanically enforced by the harness
2. **Project-2**: Context Surfaces → Rules file, skills, connectors, AX: "show don't tell" tool design
3. **Project-3**: Verification Mechanisms → Hooks, typed output, failure classes, ratchet habit
4. **Project-4**: Correction Habit → Recovery procedures, ratchet documentation, permanent fixes
5. **Project-5**: Escalation Safety Net → Human gate, traces, checkpoints, when to escalate vs. automate
6. **Project-6**: Observability & Improvement → Traces, checkpoints, control trade-off, law of diminishing returns
7. **Project-7**: Complete Harness → All 5 verbs + observability, hardened end-to-end in both Claude Code and OpenCode
8. **Project-8**: Mastery & Synthesis → Integration, self-assessment, next steps after harness engineering

## The Five Verbs (Projects 1-5)

| Verb | Project | Key Principle |
|------|---------|---------------|
| **Constrain** | Project-1 | "No, you cannot do that" — hard limits the agent cannot cross |
| **Inform** | Project-2 | "Here's what you need to do it" — context surfaces and tool design |
| **Verify** | Project-3 | "Show me you did it right" — proof before work counts |
| **Correct** | Project-4 | "Fix the system so it doesn't recur" — permanent mistake prevention |
| **Escalate** | Project-5 | "When in doubt, involve a human" — safety net when unsure |

### The 6th Surface (Project 6):

| Surface | Project | Key Principle |
|---------|---------|---------------|
| **Observability** | Project-6 | "Let me see what happened" — diagnostics and improvement |

### The Complete Harness (Projects 7):

| Project | Focus |
|---------|-------|
| **Project-7** | All 5 verbs + observability, hardened end-to-end in both Claude Code and OpenCode |

### The Capstone (Project 8):

| Project | Focus |
|---------|-------|
| **Project-8** | Mastery & synthesis — assessment and next steps after harness engineering |

## Quick Start

Each project is a self-contained directory that can be opened in OpenCode:

```bash
# Start OpenCode in a project directory
opencode

# Or view the project files
cd Project-1
opencode run "Read ./README.md"
```

## Repository Layout

```
Harness Engineering/
├── README.md                                          ← This file
├── Project-1/                                         ← Constrain verb (4 files)
│   ├── .env                                          ← Deny rule target
│   ├── opencode.json                                 ← V1 permission rules
│   ├── README.md                                     ← Project overview
│   └── demo-harness.md                               ← Constraint demonstration
├── Project-2/                                         ← Inform verb (4 files)
│   ├── .env                                          ← Inform theme test file
│   ├── opencode.json                                 ← Permission rules config
│   ├── README.md                                     ← Project overview & learnings
│   └── demo-harness.md                               ← Inform verb demonstration
├── Project-3/                                         ← Verify verb (4 files)
│   ├── .env                                          ← Verify theme test file
│   ├── opencode.json                                 ← Permission rules config
│   ├── README.md                                     ← Project overview & learnings
│   └── demo-harness.md                               ← Verify verb demonstration
├── Project-4/                                         ← Correct verb (4 files)
│   ├── .env                                          ← Correct theme test file
│   ├── opencode.json                                 ← Permission rules config
│   ├── README.md                                     ← Project overview & learnings
│   └── demo-harness.md                               ← Correct verb demonstration
├── Project-5/                                         ← Escalate verb (4 files)
│   ├── .env                                          ← Escalate theme test file
│   ├── opencode.json                                 ← Permission rules config
│   ├── README.md                                     ← Project overview & learnings
│   └── demo-harness.md                               ← Escalate verb demonstration
├── Project-6/                                         ← Observability (4 files)
│   ├── .env                                          ← Observability theme test file
│   ├── opencode.json                                 ← Permission rules config
│   ├── README.md                                     ← Project overview & learnings
│   └── demo-harness.md                               ← Observability demonstration
├── Project-7/                                         ← Complete harness (4 files)
│   ├── .env                                          ← Complete harness test file
│   ├── opencode.json                                 ← Permission rules config
│   ├── README.md                                     ← Project overview & learnings
│   └── demo-harness.md                               ← Complete harness demonstration
└── Project-8/                                         ← Mastery (4 files)
    ├── .env                                          ← Mastery theme test file
    ├── opencode.json                                 ← Permission rules config
    ├── README.md                                     ← Mastery assessment & next steps
    └── demo-harness.md                               ← Capstone integration
```

## Learning Path

**Beginner**: Start with Project-1 and work through in order.

**Developing**: Build projects 1-6, then assess your mastery with Project-8.

**Proficient**: All projects complete → Mode 2: Manufacturing course, or build personal agent harnesses.

**Master**: Consider Forward Deployed Engineer certification, or teach harness engineering to others.

## Course Reference

This repository implements the [Harness Engineering: A Crash Course](https://agentfactory.panaversity.org/docs/harness-engineering-crash-course) from the [AI Agent Factory](https://agentfactory.panaversity.org). The course covers 12 concepts across 6 parts, with 8 practice projects ranging from easy to hard.

**Key course principle**: **Agent = Model + Harness** - the model supplies intelligence; the harness turns that intelligence into something reliable.

## License

This repository is for educational purposes, following the [AI Agent Factory](https://agentfactory.panaversity.org) crash course curriculum.

---

**Built with OpenCode. Demonstrating that reliable agent harnesses are engineered, not hoped for.**