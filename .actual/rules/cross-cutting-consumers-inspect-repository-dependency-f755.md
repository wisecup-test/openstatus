---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-f755
description: "Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Consumers Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: f755c32e-0e91-49b2-80db-4883ffee588a
    status: accepted
    candidate_group_id: ac7c8bf1-716e-4288-bcba-c365b3097035
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Consumers Inspect Repository Dependency Manifest Lock

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all domain services, handlers, and business logic modules that perform reads, writes, updates, or transactional mutations on persistent relational data.

### Rules

- **R-DB-001** MUST: Consumers MUST inspect the repository dependency manifest and lock artifact to resolve the exact locked version of the database module before calling its public interfaces.

### Verify

```bash
# Inspect the project configuration to discover the automated static analysis script, then execute it to ensure all database imports originate from the designated module.
# Locate and run the test suite defined in the project configuration to verify database transactions and query operations function against the canonical schema.
```

**Accept when:**
- Static analysis verifies zero direct database connections or duplicate table definitions exist outside the designated database module.
- All service-layer data mutations and queries execute successfully within automated test suites without schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting and static analysis enforce import paths across service packages.
</enforcement>