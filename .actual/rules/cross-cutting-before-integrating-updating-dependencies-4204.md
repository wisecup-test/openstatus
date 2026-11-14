---
version: 2
name: cross-cutting-before-integrating-updating-dependencies-4204
description: "Adoption of @/libs/clients Internal Module for Centralized Client Management: Before Integrating Updating Dependencies Consumed Client"
metadata:
  actual:
    adr_id: 4204afe3-d20c-4459-9a7f-7096faff4eaa
    status: accepted
    candidate_group_id: 626a51a8-47cd-4a1f-a043-5bbe0e8aeb2b
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @/libs/clients Internal Module for Centralized Client Management: Before Integrating Updating Dependencies Consumed Client

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients.

### Rules

- **R-CLIENT-001** MUST: Before integrating or updating dependencies consumed by client abstractions, developers MUST inspect the repository lock resolution artifact to identify the exact resolved version and verify supported API surface against authoritative documentation.
- **R-CLIENT-002** MUST: All server route handlers, protocol adapters, background agent runners, and store implementations requiring infrastructure or service clients MUST delegate client access to the centralized internal module @/libs/clients.
- **R-CLIENT-003** MUST: Expose pre-configured singleton client instances from the central module to ensure connection reuse across request lifecycles.
- **R-CLIENT-004** MUST: Ensure that client configurations reference validated runtime environment schemas rather than reading raw process environment variables within route handlers.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce compliance.
</enforcement>