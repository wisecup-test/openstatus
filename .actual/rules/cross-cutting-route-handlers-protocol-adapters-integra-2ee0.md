---
version: 2
name: cross-cutting-route-handlers-protocol-adapters-integra-2ee0
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Route Handlers Protocol Adapters Integration Stores"
metadata:
  actual:
    adr_id: 2ee08031-53ec-45a8-b6ca-313e24e8ac3b
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Route Handlers Protocol Adapters Integration Stores

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

### Rules

- **R-CLIENT-001** MUST: Route handlers, protocol adapters, integration stores, and agent runners MUST import backing infrastructure and service client instances exclusively from @/libs/clients.

### Verify

```bash
# Discover and run repository architectural lint scripts to confirm all client imports originate from @/libs/clients
# Run the project test suite to verify route and adapter integration with client instances
```

**Accept when:**
- All imports of backing infrastructure clients across server route files and adapters resolve to @/libs/clients.
- No route, adapter, or store file instantiates external connection drivers or client libraries directly.
- The repository test suite and module verification checks pass without architectural import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks inspecting import paths and peer code reviews.
</enforcement>