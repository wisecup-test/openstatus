---
version: 2
name: cross-cutting-before-integrating-updating-dependencies-b920
description: "Adoption of @openstatus/ui Shared Component Library: Before Integrating Updating Dependencies Openstatus Consumers"
metadata:
  actual:
    adr_id: b9200fa3-8bb6-49ea-8cbb-e067e90ed2ee
    status: accepted
    candidate_group_id: 5002540a-ce29-430d-b1f1-772828a27f05
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Before Integrating Updating Dependencies Openstatus Consumers

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPEN-001** MUST: Before integrating or updating dependencies from @openstatus/ui, consumers MUST inspect the repository lock artifact to resolve the exact dependency version and verify API compatibility against current library specifications.

### Verify

```bash
# Discover and execute repository type-checking, linting, and automated test commands
# (Commands must be derived from the project repository configuration)
```

**Accept when:**
- Static analysis and type checking pass without errors or unresolved module references across all consuming components.
- Interactive form and selection controls render properly and respond to state changes without console warnings or unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>