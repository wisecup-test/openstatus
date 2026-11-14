---
version: 2
name: cross-cutting-status-page-subscriber-lifecycle-operati-8b28
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Status Page Subscriber Lifecycle Operations Token"
metadata:
  actual:
    adr_id: 8b2865da-8d88-4170-9d18-ffc66204e86c
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Status Page Subscriber Lifecycle Operations Token

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SUB-001** MUST: All status page subscriber lifecycle operations, token validations, and subscriber listings MUST be delegated to the adopted @openstatus/services/page-subscriber module rather than implemented directly within API router procedures.

### Verify

```bash
# Discover and run project static analysis and type checking script
# Discover and run project test suite covering router endpoints and domain service integration
```

**Accept when:**
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and type checks within the CI pipeline, and peer code review.
</enforcement>