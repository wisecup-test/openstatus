---
version: 2
name: cross-cutting-routers-interacting-page-subscriber-reco-2bd0
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Routers Interacting Page Subscriber Records Utilize"
metadata:
  actual:
    adr_id: 2bd05f9e-0cbc-4517-85b7-33d219499e68
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Routers Interacting Page Subscriber Records Utilize

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API procedures and routers handling status page subscriber subscriptions, token verification, and lifecycle workflows.

### Rules

- **R-SUB-001** SHOULD: API routers interacting with page subscriber records SHOULD utilize domain queries such as getSubscriberByToken and listPageSubscribers to maintain a consistent subscriber lifecycle boundary.

### Verify

```bash
# Discover and run the project static analysis and type checking script
# Discover and run the project test suite covering router endpoints and domain service integration
```

**Accept when:**
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, type checks within CI, and peer code review.
</enforcement>