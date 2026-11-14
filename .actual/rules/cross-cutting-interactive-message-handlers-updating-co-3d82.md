---
version: 2
name: cross-cutting-interactive-message-handlers-updating-co-3d82
description: "@slack/web-api Integration Client Adoption: Interactive Message Handlers Updating Conversation Threads"
metadata:
  actual:
    adr_id: 3d82b9c5-a094-4173-b946-64096f88212e
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: Interactive Message Handlers Updating Conversation Threads

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** SHOULD: Interactive message handlers updating conversation threads via chat.update SHOULD clear interactive block structures and display deterministic terminal outcome text when actions complete or expire.

### Verify

```bash
# Discover the test automation task in the project configuration manifest and execute integration test suites targeting external messaging routes.
# Identify the repository type checking and static analysis verification scripts and run them across all integration boundary modules.
```

**Accept when:**
- Integration test suites verify that interactive messaging routes successfully invoke client update and publish methods with valid payloads.
- Confirmation store tests confirm that pending actions are successfully recorded, consumed exactly once, and rejected upon expiration.
- Static type analysis and schema validations pass with zero diagnostic errors across all integration boundary modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>