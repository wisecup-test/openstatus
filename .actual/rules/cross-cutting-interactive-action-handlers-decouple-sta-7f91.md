---
version: 2
name: cross-cutting-interactive-action-handlers-decouple-sta-7f91
description: "@slack/web-api Integration Client Adoption: Interactive Action Handlers Decouple Stateful Operation"
metadata:
  actual:
    adr_id: 7f911fa1-4fb4-48ce-86d7-ea171495abe7
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: Interactive Action Handlers Decouple Stateful Operation

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all server routes and modules managing interactive messaging integration, view publishing, and interaction handling across platform boundaries.

### Rules

- **R-SLACK-001** MUST: Interactive action handlers MUST decouple stateful operation execution from webhook receipt by staging pending actions in a confirmation store and consuming each action record exactly once upon execution.

### Verify

```bash
# Discover and execute the test automation task for integration tests targeting external messaging routes
# Discover and run repository type checking and static analysis verification scripts across integration boundary modules
```

**Accept when:**
- Integration test suites verify that interactive messaging routes successfully invoke client update and publish methods with valid payloads.
- Confirmation store tests confirm that pending actions are successfully recorded, consumed exactly once, and rejected upon expiration.
- Static type analysis and schema validations pass with zero diagnostic errors across all integration boundary modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test validation suites and peer code reviews.
</enforcement>