---
version: 2
name: cross-cutting-view-definitions-published-via-views-8463
description: "@slack/web-api Integration Client Adoption: View Definitions Published Via Views Publish"
metadata:
  actual:
    adr_id: 8463bb57-16a6-4317-b062-f747be55f746
    status: accepted
    candidate_group_id: 668877c4-afd0-4ee3-8752-56fcdbc2c789
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# @slack/web-api Integration Client Adoption: View Definitions Published Via Views Publish

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLACK-001** SHOULD: View definitions published via views.publish SHOULD encapsulate structural block generation within dedicated presentation builder functions to isolate view layout from route orchestration.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>