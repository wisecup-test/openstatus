---
version: 2
name: cross-cutting-client-abstraction-module-encapsulate-co-7c97
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Client Abstraction Module Encapsulate Connection Lifecycle"
metadata:
  actual:
    adr_id: 7c9793a9-e81a-4275-a8a8-20b72e1fe095
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Client Abstraction Module Encapsulate Connection Lifecycle

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

### Rules

- **R-CLI-001** SHOULD: The client abstraction module SHOULD encapsulate connection lifecycle configuration, credentials resolution, and base timeout handling for all exported client instances.

### Verify

```bash
# Discover and run the repository architectural lint scripts to confirm all client imports originate from the designated module.
# Run the project test suite to verify route and adapter integration with client instances.
```

**Accept when:**
- All imports of backing infrastructure clients across server route files and adapters resolve to @/libs/clients.
- No route, adapter, or store file instantiates external connection drivers or client libraries directly.
- The repository test suite and module verification checks pass without architectural import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests containing direct client driver instantiations outside the designated module will be blocked during automated verification.
</enforcement>