---
version: 2
name: cross-cutting-before-implementing-modifying-components-4e79
description: "@slack/web-api Integration Client Adoption: Before Implementing Modifying Components That Integrate"
metadata:
  actual:
    adr_id: 4e7950ca-c234-4576-bda7-52e4c2b46b05
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: Before Implementing Modifying Components That Integrate

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all server routes, modules, and asynchronous interactive workflows managing messaging integration, view publishing, and interaction handling across platform boundaries.

### Rules

- **R-SLACK-001** MUST: Before implementing or modifying components that integrate with @slack/web-api, engineers MUST discover the workspace dependency manifest and package resolution lock artifact to identify and verify the exact resolved dependency version.

### Verify

```bash
# Discover and execute the test automation task in the project configuration manifest targeting external messaging routes
# Identify repository type checking and static analysis verification scripts and run them across all integration boundary modules
```

**Accept when:**
- Integration test suites verify that interactive messaging routes successfully invoke client update and publish methods with valid payloads.
- Confirmation store tests confirm that pending actions are successfully recorded, consumed exactly once, and rejected upon expiration.
- Static type analysis and schema validations pass with zero diagnostic errors across all integration boundary modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>