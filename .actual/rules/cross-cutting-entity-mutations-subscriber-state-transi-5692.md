---
version: 2
name: cross-cutting-entity-mutations-subscriber-state-transi-5692
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Entity Mutations Subscriber State Transitions Execute"
metadata:
  actual:
    adr_id: 5692bcfc-3ea7-439e-913a-11be36970a0b
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Entity Mutations Subscriber State Transitions Execute

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: Entity mutations and subscriber state transitions MUST execute through schema-bound update builders provided by @openstatus/db rather than raw query strings.

### Verify

```bash
# Discover and run the repository static analysis, linting, type-checking, and test scripts
# 1. Verify persistence imports originate exclusively from the authorized shared database module.
# 2. Execute type-checking to ensure all schema entities conform to the shared database module definitions.
# 3. Run the automated test suite to confirm database query and mutation operations fulfill schema contracts.
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type checking, and workspace-wide validation must be enforced in the continuous integration pipeline.
</enforcement>