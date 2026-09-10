# Project 1: Harness Constraints

## Objective
Set up basic harness constraint mechanisms (permission rules) to demonstrate the "constrain" verb from the harness engineering crash course.

## What This Project Covers
- **Constrain verb**: Limit what the agent can do through permission rules
- **Allow/Ask/Deny**: The three responses for action classification
- **Blast radius**: Sorting actions by potential damage
- **Guardrails**: Mechanically enforced rules (not prompt-dependent)

## Setup
1. Permission rules created in `opencode.json` or `settings.json`
2. Deny rules for high-blast-radius actions
3. Allow rules for low-risk actions
4. Ask rules for medium-risk actions requiring human confirmation

## Key Learnings
- Constraint is mechanically enforced by the harness, not by asking nicely in prompts
- Deny rules beat ask rules, which beat allow rules
- Sort actions by blast radius, not frequency
- "A guardrail lives in the harness, never in the prompt"