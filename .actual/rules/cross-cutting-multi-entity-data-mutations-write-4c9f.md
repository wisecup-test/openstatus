---
version: 2
name: cross-cutting-multi-entity-data-mutations-write-4c9f
description: "Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Multi Entity Data Mutations Write Operations"
metadata:
  actual:
    adr_id: 4c9f5e78-d5b6-432e-a160-e556b4cf666b
    status: accepted
    candidate_group_id: ac7c8bf1-716e-4288-bcba-c365b3097035
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Multi Entity Data Mutations Write Operations

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Multi-entity data mutations and write operations MUST execute within a managed transaction context handle provided by the database client.

### Verify

```bash
# Inspect project configuration to discover the automated static analysis script and run it
# Locate and run the test suite defined in the project configuration
```

**Accept when:**
- Static analysis verifies zero direct database connections or duplicate table definitions exist outside the designated database module.
- All service-layer data mutations and queries execute successfully within automated test suites without schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>