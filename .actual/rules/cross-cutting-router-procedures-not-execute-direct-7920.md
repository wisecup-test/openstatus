---
version: 2
name: cross-cutting-router-procedures-not-execute-direct-7920
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Router Procedures Not Execute Direct Database"
metadata:
  actual:
    adr_id: 79209cc5-ebb9-49e8-8b1c-e5ac1abd5af2
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Router Procedures Not Execute Direct Database

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SUB-001** MUST_NOT: API router procedures MUST_NOT execute direct database mutations or raw database queries on subscriber entities outside the @openstatus/services/page-subscriber interface boundary.

### Verify

```bash
# Discover and run the project static analysis and type checking script
# Discover and run the project test suite covering router endpoints and domain service integration
```

**Accept when:**
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>