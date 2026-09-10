# Project 2: Harness Inform

## Objective
Set up harness inform mechanisms (context surfaces, rules file, skills) to demonstrate the "inform" verb from the harness engineering crash course.

## What This Project Covers
- **Inform verb**: Give the agent what it needs to do the job right
- **Rules file**: What is always true in this project
- **Skills**: Saved instructions loaded when task matches
- **Connectors**: What the agent can reach and how
- **AX (Agent Experience)**: Designing tools and errors for the agent

## Setup
1. Rules file (`AGENTS.md` or project conventions) answers: *what is always true here?*
2. Skills loaded only when task matches
3. Connectors (MCP servers) attached as needed
4. Tool design follows AX principles

## Key Learnings
- Information flow is as important as constraint flow
- The rules file is read every run - keep it short
- Skills free detail until needed (loaded on demand)
- Tool design affects agent performance more than prompt tweaks
- "Show, don't tell" - design tools the agent can actually use