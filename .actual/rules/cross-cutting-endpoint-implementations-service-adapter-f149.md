---
version: 2
name: cross-cutting-endpoint-implementations-service-adapter-f149
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Endpoint Implementations Service Adapters Not Instantiate"
metadata:
  actual:
    adr_id: f1498b28-505f-4952-b8cb-f2ffba4b019a
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Endpoint Implementations Service Adapters Not Instantiate

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

### Rules

- **R-CLIENT-001** MUST_NOT: Endpoint implementations and service adapters MUST NOT instantiate independent client connections or initialize underlying client drivers directly within handler scopes.

### Verify

```bash
# Discover and run the repository architectural lint scripts to confirm all client imports originate from the designated module
# Run the project test suite to verify route and adapter integration with client instances
```

**Accept when:**
- All imports of backing infrastructure clients across server route files and adapters resolve to @/libs/clients.
- No route, adapter, or store file instantiates external connection drivers or client libraries directly.
- The repository test suite and module verification checks pass without architectural import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct client driver instantiations outside the designated module will be blocked during automated verification.
</enforcement>