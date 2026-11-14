---
version: 2
name: cross-cutting-router-procedures-invoking-openstatus-se-b89d
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Router Procedures Invoking Openstatus Services Page"
metadata:
  actual:
    adr_id: b89dbfd7-960a-4381-bb5a-0978f41f8f9d
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Router Procedures Invoking Openstatus Services Page

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for API procedures and routers handling status page subscriber subscriptions, token verification, and lifecycle workflows.

### Rules

- **R-SUB-001** MUST: Router procedures invoking @openstatus/services/page-subscriber functions MUST convert inbound request context using a service context adapter and pass structured inputs conforming to the service contract.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify interface compatibility with the subscriber service module.
# Discover and run the project test suite covering router endpoints and domain service integration.
```

**Accept when:**
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>