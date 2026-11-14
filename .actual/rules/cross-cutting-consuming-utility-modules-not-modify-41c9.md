---
version: 2
name: cross-cutting-consuming-utility-modules-not-modify-41c9
description: "Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Utility Modules Not Modify Extend"
metadata:
  actual:
    adr_id: 41c91055-3f4d-458b-bcf5-4d2cd0e4a92c
    status: accepted
    candidate_group_id: 20d7a85b-14ec-4581-82b0-2748cc7d83d2
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consuming Utility Modules Not Modify Extend

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for internal workspace packages and utility modules that operate on database-backed domain entities.

### Rules

- **R-DB-001** MUST_NOT: Consuming utility modules MUST NOT modify or extend imported schema definitions locally in ways that violate central schema contracts.
- **R-DB-002** MANDATORY: Verify workspace dependency manifests declare proper internal package references for the centralized database schema module before importing schema members.
- **R-DB-003** MANDATORY: Consume shared schema types directly rather than asserting structural type equivalence in local utility functions.

### Verify

```bash
# Discover and run the repository type check script declared in the project configuration manifest across all packages
# (Command must be derived from project repository configuration)

# Discover and execute the repository static analysis and test validation suites to verify cross-package schema integration
# (Command must be derived from project repository configuration)
```

**Accept when:**
- Static analysis and type checking pass across workspace packages consuming the centralized schema package without unresolved module errors.
- Domain utility logic compiles against the shared database schema definitions with no conflicting local data contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type checking, and peer code review enforce cross-package import boundaries and block unapproved schema imports or duplicated data models.
</enforcement>