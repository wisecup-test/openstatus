---
version: 2
name: cross-cutting-relational-read-queries-across-related-c074
description: "Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Relational Read Queries Across Related Entities"
metadata:
  actual:
    adr_id: c0742450-4c46-4e95-a91c-14661e71c852
    status: accepted
    candidate_group_id: ac7c8bf1-716e-4288-bcba-c365b3097035
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Relational Read Queries Across Related Entities

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all domain services, handlers, and business logic modules that perform reads, writes, updates, or transactional mutations on persistent relational data.

### Rules

- **R-OPENSTATUS-001** SHOULD: Relational read queries across related entities SHOULD utilize the relational query builder interface provided by the database client rather than unmanaged raw query strings.

### Verify

```bash
# Inspect the project configuration to discover the automated static analysis script, then execute it to ensure all database imports originate from the designated module.
# Locate and run the test suite defined in the project configuration to verify database transactions and query operations function against the canonical schema.
```

**Accept when:**
- Static analysis verifies zero direct database connections or duplicate table definitions exist outside the designated database module.
- All service-layer data mutations and queries execute successfully within automated test suites without schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, static analysis, and peer code review enforce these policies.
</enforcement>