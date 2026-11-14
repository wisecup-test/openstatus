---
version: 2
name: cross-cutting-consuming-modules-isolate-schema-depende-443f
description: "Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Modules Isolate Schema Dependent Conversions"
metadata:
  actual:
    adr_id: 443f45cd-04aa-40f8-97a3-dcc15cd2bb9f
    status: accepted
    candidate_group_id: 20d7a85b-14ec-4581-82b0-2748cc7d83d2
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Modules Isolate Schema Dependent Conversions

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for internal workspace packages and utility modules that operate on database-backed domain entities.

### Rules

- **R-SCHEMA-001** SHOULD: Consuming modules isolate schema-dependent conversions within dedicated boundary utilities to minimize ripple effects from schema migrations.
- **R-SCHEMA-002** MANDATORY: Verify that workspace dependency manifests declare proper internal package references for the database schema package before importing schema members.
- **R-SCHEMA-003** MANDATORY: Consume shared schema types directly rather than asserting structural type equivalence in local utility functions.

### Verify

```bash
# Discover and execute the repository type check script declared in the project configuration manifest across all packages
# Discover and execute the repository static analysis and test validation suites to verify cross-package schema integration
```

**Accept when:**
- Static analysis and type checking pass across workspace packages consuming the shared schema module without unresolved module errors.
- Domain utility logic compiles against the shared database schema definitions with no conflicting local data contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>