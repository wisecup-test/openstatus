---
version: 2
name: cross-cutting-external-messaging-integration-boundarie-4b3e
description: "@slack/web-api Integration Client Adoption: External Messaging Integration Boundaries Communicating Interactive"
metadata:
  actual:
    adr_id: 4b3ee69f-27e1-4c41-a85c-c06ca41134cc
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: External Messaging Integration Boundaries Communicating Interactive

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all server routes and modules managing interactive messaging integration, view publishing, and interaction handling across platform boundaries.

### Rules

- **R-SLACK-001** MUST: All external messaging integration boundaries communicating with the interactive messaging platform MUST route view publication and message update operations exclusively through the @slack/web-api client.

### Verify

```bash
# Discover and execute test automation suites targeting external messaging routes
# Discover and run repository type checking and static analysis verification scripts across integration boundary modules
```

**Accept when:**
- Integration test suites verify that interactive messaging routes successfully invoke client update and publish methods with valid payloads.
- Confirmation store tests confirm that pending actions are successfully recorded, consumed exactly once, and rejected upon expiration.
- Static type analysis and schema validations pass with zero diagnostic errors across all integration boundary modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test validation suites and peer code review.
</enforcement>