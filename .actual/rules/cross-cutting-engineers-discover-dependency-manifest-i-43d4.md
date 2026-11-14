---
version: 2
name: cross-cutting-engineers-discover-dependency-manifest-i-43d4
description: "Adoption of @openstatus/services as the Shared Domain Service Module: Engineers Discover Dependency Manifest Inspect Authoritative"
metadata:
  actual:
    adr_id: 43d45ccb-f184-43bc-80d7-e1bed6ad3967
    status: accepted
    candidate_group_id: 59a6b1ea-bddf-485b-a89f-dea1337ef130
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @openstatus/services as the Shared Domain Service Module: Engineers Discover Dependency Manifest Inspect Authoritative

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server route handlers, webhook receivers, and third-party integration adapters executing domain actions.

### Rules

- **R-OPENSTATUS-001** MUST: Engineers MUST discover the dependency manifest and inspect the authoritative repository lock artifact to verify the exact resolved version of @openstatus/services before consuming or altering its exported interfaces.

### Verify

```bash
# Discover the root project manifest and execute the designated workspace test suite covering route integration and service adapter modules.
# Discover the repository linter and type-checking scripts from the workspace configuration to validate import boundary compliance across route handlers.
```

**Accept when:**
- All server route handlers and protocol adapters route domain operations through @openstatus/services rather than executing domain logic inline.
- Project verification scripts and static analysis suites complete with zero boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code reviews verify import boundaries between route handlers and internal database schema packages.
</enforcement>