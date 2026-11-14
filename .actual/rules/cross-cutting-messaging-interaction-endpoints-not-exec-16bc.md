---
version: 2
name: cross-cutting-messaging-interaction-endpoints-not-exec-16bc
description: "@slack/web-api Integration Client Adoption: Messaging Interaction Endpoints Not Execute Stateful"
metadata:
  actual:
    adr_id: 16bc7416-3785-426f-be48-dc3001fd240a
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: Messaging Interaction Endpoints Not Execute Stateful

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all server routes and modules managing interactive messaging integration, view publishing, and interaction handling across platform boundaries.

### Rules

- **R-SLACK-001** MUST_NOT: Messaging interaction endpoints MUST_NOT execute stateful domain mutations directly from inbound payloads without prior schema validation and confirmation state resolution.

### Verify

```bash
# Discover and run the test automation task in the project configuration manifest targeting external messaging routes
# Discover and run repository type checking and static analysis verification scripts across all integration boundary modules
```

**Accept when:**
- Integration test suites verify that interactive messaging routes successfully invoke client update and publish methods with valid payloads.
- Confirmation store tests confirm that pending actions are successfully recorded, consumed exactly once, and rejected upon expiration.
- Static type analysis and schema validations pass with zero diagnostic errors across all integration boundary modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>