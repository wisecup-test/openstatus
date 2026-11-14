---
version: 2
name: cross-cutting-client-module-provide-stubbed-factories-91a7
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Client Module Provide Stubbed Factories Test"
metadata:
  actual:
    adr_id: 91a7f8a9-20f8-409f-913c-ecedbd111c6d
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Client Module Provide Stubbed Factories Test

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

### Rules

- **R-CLIENT-001** MAY: The client module MAY provide stubbed client factories for test environments that conform to the production client interface contract.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>