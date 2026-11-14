---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-m-f1f0
description: "Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Consumers Inspect Workspace Dependency Manifests Resolved"
metadata:
  actual:
    adr_id: f1f0d0c8-65da-4f84-a35e-f58095c84a5d
    status: accepted
    candidate_group_id: e2d352d6-144b-439f-aba4-818e08020193
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Shared Database Schema and Domain Constants: Consumers Inspect Workspace Dependency Manifests Resolved

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for any application tier defining or validating database entity structures, including client-side form controllers, data table components, and backend route handlers interacting with domain entities.

### Rules

- **R-OPENSTATUS-001** MUST: Consumers MUST inspect the workspace dependency manifests and resolved lock artifacts to confirm module compatibility before integrating shared schema definitions.

### Verify

```bash
# Discover and run the project static type analysis script to verify all schema imports and type usages across the workspace.
# Discover and execute the workspace test suite to confirm that validation rules and domain enums match expected behavior across all consuming modules.
```

**Accept when:**
- The repository type checker completes without errors across all modules importing the shared schema definitions.
- All form validation and data presentation tests execute successfully without schema mismatch failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>