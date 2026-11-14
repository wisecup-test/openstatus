---
version: 2
name: cross-cutting-channel-delivery-handlers-performing-sub-cd57
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Channel Delivery Handlers Performing Subscriber State"
metadata:
  actual:
    adr_id: cd572e25-996a-4ae1-ac40-09f317274efe
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Channel Delivery Handlers Performing Subscriber State

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CHNL-001** SHOULD: Channel delivery handlers performing subscriber state updates supply explicit timestamp values for auditability and state synchronization.
- **R-CHNL-002** MANDATORY: Import database query clients and relational schemas strictly from the authorized exports of the shared database module.
- **R-CHNL-003** MANDATORY: Structure relational queries with deterministic ordering and explicit relational inclusions to ensure idempotency across asynchronous event processing.
- **R-CHNL-004** MANDATORY: Execute discovery of project dependencies, build tools, lock files, and documentation before writing code that uses a versioned library.

### Verify

```bash
# Discover and run the repository static analysis and linting scripts to verify persistence imports originate exclusively from the authorized shared database module
# Discover and execute the automated test suite to confirm database query and mutation operations fulfill schema and ordering contracts
# Execute the project type-checking script to ensure all schema entities and query parameter objects conform to shared database module definitions
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>