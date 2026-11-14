---
version: 2
name: cross-cutting-downstream-route-modules-consume-client-5345
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Downstream Route Modules Consume Client Instances"
metadata:
  actual:
    adr_id: 53455c0d-5e0c-4f97-819a-cc2e712e6747
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Downstream Route Modules Consume Client Instances

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

### Rules

- **R-CLI-001** SHOULD: Downstream route modules consume client instances via exported interfaces rather than relying on driver-specific ambient globals.

### Verify

```bash
# Discover and run repository architectural lint scripts to confirm all client imports originate from the designated module
# Run the project test suite to verify route and adapter integration with client instances
```

**Accept when:**
- All imports of backing infrastructure clients across server route files and adapters resolve to @/libs/clients.
- No route, adapter, or store file instantiates external connection drivers or client libraries directly.
- The repository test suite and module verification checks pass without architectural import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct client driver instantiations outside the designated module will be blocked.
</enforcement>