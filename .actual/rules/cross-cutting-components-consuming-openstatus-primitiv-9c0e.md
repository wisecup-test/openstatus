---
version: 2
name: cross-cutting-components-consuming-openstatus-primitiv-9c0e
description: "Adoption of @openstatus/ui Shared Component Library: Components Consuming Openstatus Primitives Within Interactive"
metadata:
  actual:
    adr_id: 9c0e109d-266c-41eb-b5db-7f3f759825cf
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Components Consuming Openstatus Primitives Within Interactive

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: Components consuming @openstatus/ui primitives within interactive client boundaries MUST explicitly declare client-side rendering execution.

### Verify

```bash
# Discover and execute the repository type-checking script to verify module imports and component props
# Discover and execute the project linting and structural analysis suite
# Discover and execute the automated test runner to validate form interaction and selection state handling
```

**Accept when:**
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>