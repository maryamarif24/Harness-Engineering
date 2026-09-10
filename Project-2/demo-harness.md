# Harness Inform Demo

This project demonstrates the **inform** verb from the Harness Engineering crash course.

## Inform Verb Structure

The inform verb answers: **"What does the agent need to know to do the job right?"**

Unlike constrain (which says "no"), inform says "here's what you need." Both are mechanically enforced by the harness.

## Three Inform Surfaces

### 1. Rules File (`AGENTS.md`)

Answers: ***what is always true here?***

- Conventions, boundaries, lessons the ratchet has saved
- Read every run - every line costs tokens each beat
- Keep it short; let `/doctor`-style checkups trim what the agent can learn from codebase itself

**Example AGENTS.md content:**
```
# Project Conventions

- Tests go in `tests/` directory
- Source files in `src/` only
- Never commit directly to `main`
- Use `npm run lint` before committing
```

### 2. Skills (`SKILL.md`)

Answers: ***how do we do this specific job?***

- Loaded only when the task matches
- Detail is free until needed
- The daily-triage skill from the last course is a harness part

**Example SKILL.md content:**
```markdown
# Daily Triage Skill

When task matches "triage", load this skill:
1. Read existing test results
2. Identify failing tests
3. Report concise findings
4. Suggest targeted fixes
```

### 3. Connectors (MCP Servers)

Answers: ***what can it reach, and how?***

- Which MCP servers are attached is an inform decision
- Every attached tool is both a capability and a permission decision
- Tool supply chain must be trusted (pin versions, enforce allowlists)

## AX (Agent Experience) Principle

> **"Show, don't tell"** - Design tools the agent can actually use

### Bad Design (Prompt-Dependent)
```
# In prompt: "Read the config file carefully and understand the format"
# Agent might misread, miss, or forget
```

### Good Design (Harness-Enabled)
```
# Tool description: "Read config from ./config.json - returns JSON
# with fields: host, port, timeout, debug"
# Shape: { host: string, port: number, timeout: number, debug: boolean }
# Code validates shape, agent trusts the format
```

## Key Takeaways

1. **Information over restriction**: Inform complements constrain
2. **Rules file costs tokens every beat**: Keep it lean
3. **Skills load on demand**: Detail free until needed
4. **Tool design = AX**: Well-designed tools outperform better prompts
5. **Connectors are trust decisions**: Pin versions, enforce allowlists
6. **The ratchet applies**: Every information gap → permanent harness fix

## Demonstration

### Rules File Impact
When the agent starts, it reads `AGENTS.md` (or equivalent). Every line becomes context. Too long = wasted tokens, missed rules.

### Skills on Demand
Skills are loaded only when the task matches. This means:
- Simple tasks don't pay the complexity cost
- Complex tasks get detailed instructions
- No need to embed all knowledge in every prompt

### Connector Trust
Every MCP server attached is a trust decision:
- Pin by version so updates don't surprise
- Allowlist domains for network fences
- Review new tools before production use