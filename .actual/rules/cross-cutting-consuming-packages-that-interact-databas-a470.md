---
version: 2
name: cross-cutting-consuming-packages-that-interact-databas-a470
description: "Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Packages That Interact Database Backed"
metadata:
  actual:
    adr_id: a470bcd2-5261-451b-a707-fbcde1a37f79
    status: accepted
    candidate_group_id: 20d7a85b-14ec-4581-82b0-2748cc7d83d2
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Packages That Interact Database Backed

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for internal workspace packages and utility modules that operate on database-backed domain entities.

### Rules

- **R-DB-001** MUST: Consuming packages that interact with database-backed incident entities MUST import their schema types from @openstatus/db/src/schema rather than redefining local structural duplicates.

### Verify

```bash
# Discover the repository type check script declared in the project configuration manifest and execute it across all packages.
# Discover and execute the repository static analysis and test validation suites to verify cross-package schema integration.
```

**Accept when:**
- Static analysis and type checking pass across workspace packages consuming @openstatus/db/src/schema without unresolved module errors.
- Domain utility logic compiles against the shared database schema definitions with no conflicting local data contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code review verify cross-package import boundaries.
</enforcement>