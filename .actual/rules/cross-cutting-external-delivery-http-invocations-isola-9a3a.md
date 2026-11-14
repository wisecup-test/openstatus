---
version: 2
name: cross-cutting-external-delivery-http-invocations-isola-9a3a
description: "Adoption of @openstatus/notification-base for Notification Provider Integration: External Delivery Http Invocations Isolated Provider"
metadata:
  actual:
    adr_id: 9a3a3b70-500c-4f0d-ab51-a41a3a05a9bf
    status: accepted
    candidate_group_id: f38733e0-f502-4a66-81d9-7b70c2173a8d
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Provider Integration: External Delivery Http Invocations Isolated Provider

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** SHOULD: External delivery HTTP invocations SHOULD be isolated to provider dispatch boundaries after payload validation completes.

### Verify

```bash
# Discover and run the project's workspace-wide static type-checking script to confirm full contract adherence with @openstatus/notification-base.
# Discover and execute the test suite corresponding to notification delivery integrations to verify payload parsing and external dispatch behaviors.
```

**Accept when:**
- All notification provider packages compile without type errors against the imported base contract.
- Unit and integration tests for notification dispatch pass successfully, confirming valid schema parsing and external request dispatching.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>