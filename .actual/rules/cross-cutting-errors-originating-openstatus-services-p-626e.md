---
version: 2
name: cross-cutting-errors-originating-openstatus-services-p-626e
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Errors Originating Openstatus Services Page Subscriber"
metadata:
  actual:
    adr_id: 626e3c59-2010-4cf9-8af7-1a7370ec5303
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Errors Originating Openstatus Services Page Subscriber

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API procedures and routers handling status page subscriber subscriptions, token verification, and lifecycle workflows.

### Rules

- **R-ADR-001** MUST: Errors originating from @openstatus/services/page-subscriber invocations MUST be caught and mapped through standardized error adapter utilities before propagating to client boundaries.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify interface compatibility
# Discover and run the project test suite covering router endpoints and domain service integration
```

**Accept when:**
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>