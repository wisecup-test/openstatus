---
version: 2
name: cross-cutting-domain-services-requiring-persistent-dat-8ec8
description: "Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Domain Services Requiring Persistent Data Access"
metadata:
  actual:
    adr_id: 8ec81d3e-2f10-47d6-918c-55623c83d096
    status: accepted
    candidate_group_id: ac7c8bf1-716e-4288-bcba-c365b3097035
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Domain Services Requiring Persistent Data Access

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all domain services, handlers, and business logic modules that perform reads, writes, updates, or transactional mutations on persistent relational data.

### Rules

- **R-DB-001** MUST: Domain services requiring persistent data access MUST import database client utilities and schema entities exclusively from @openstatus/db and its schema subpaths.

### Verify

```bash
# Inspect the project configuration to discover the automated static analysis script, then execute it
# Locate and run the test suite defined in the project configuration
```

**Accept when:**
- Static analysis verifies zero direct database connections or duplicate table definitions exist outside the designated database module.
- All service-layer data mutations and queries execute successfully within automated test suites without schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, static analysis, and peer code review enforce that all database imports originate from the designated module.
</enforcement>