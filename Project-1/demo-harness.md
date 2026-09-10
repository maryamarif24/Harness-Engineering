# Harness Constraint Demo

This project demonstrates the **constrain** verb from the Harness Engineering crash course.

## Permission Rule Structure

Each rule has one of three responses:

| Response | Meaning | Example |
|----------|---------|---------|
| **Allow** | Run silently, no interruption | `Read`, `Bash(npm test *)` |
| **Ask** | Stop and request human yes/no | `Bash(*)`, `edit` |
| **Deny** | Never allow, mechanically enforced | `Bash(rm -rf *)`, `Read(./.env)` |

## Rule Priority Order

1. **Deny** always wins — if a rule matches, the action is blocked regardless of other rules
2. **Ask** — if a rule matches but doesn't deny, the agent prompts for confirmation
3. **Allow** — if a rule matches and doesn't ask/deny, the action runs silently

> **Important**: A broad `allow` cannot leak past a narrow `deny`. If both `Bash(git push *)` allow and `Bash(git push --force *)` deny exist, the deny takes precedence for force pushes.

## Blast Radius Sorting Principle

Sort actions by potential damage if they go wrong, not by frequency:

- **Low blast radius** → Allow (read source files, run tests)
- **Medium blast radius** → Ask (git pushes, file edits)
- **High blast radius** → Deny (secrets access, force push, rm -rf)

## Demo: Wall Holds

### Scenario
An agent tries to read a `.env` file containing secrets.

### Without Harness
Agent reads `.env` contents → secrets exposed (prompt "please don't" is ignored)

### With Harness (this project)
1. Deny rule: `Read(./.env)` 
2. Agent cannot access `.env` — mechanically enforced
3. Secret never reaches the agent

### Test
```bash
# Try to read .env - should be blocked by deny rule
# Expected: Permission denied, secret stays protected
```

## Network Fence Demo

### Scenario
Agent tries to fetch external resources.

### With Network Sandbox
- Empty host allowlist blocks all outbound network
- `curl https://example.com` fails with network error
- No data can leak out

### Without Network Fence
- Agent can reach outside servers
- Injected instructions can exfiltrate data

## Key Takeaways

1. **Constraint over persuasion**: Rules enforce; prompts don't
2. **Defense in depth**: Multiple fences (deny rules + sandbox + network fence)
3. **The ratchet habit**: Every mistake → permanent harness fix
4. **Guardrails live in harness**: Steel barrier, not road sign