---
version: 2
name: cross-cutting-consumers-discover-workspace-dependency-3f37
description: "Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consumers Discover Workspace Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 3f377041-33da-49e4-a7bd-e07f2556fbdd
    status: accepted
    candidate_group_id: 20d7a85b-14ec-4581-82b0-2748cc7d83d2
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adoption of @openstatus/db/src/schema Internal Database Schema Module: Consumers Discover Workspace Dependency Resolution Artifact

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for internal workspace packages and utility modules that operate on database-backed domain entities.

### Rules

- **R-ADR-001** MUST: Consumers MUST discover the workspace dependency resolution artifact and verify the locked version and integrity of all workspace packages prior to referencing their exports.

### Verify

```bash
# Discover the repository type check script declared in the project configuration manifest and execute it across all packages.
# Discover and execute the repository static analysis and test validation suites to verify cross-package schema integration.
```

**Accept when:**
- Static analysis and type checking pass across workspace packages consuming @openstatus/db/src/schema without unresolved module errors.
- Domain utility logic compiles against the shared database schema definitions with no conflicting local data contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>