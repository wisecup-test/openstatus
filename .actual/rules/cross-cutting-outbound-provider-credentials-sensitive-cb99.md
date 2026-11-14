---
version: 2
name: cross-cutting-outbound-provider-credentials-sensitive-cb99
description: "Adoption of @openstatus/notification-base for Notification Provider Integration: Outbound Provider Credentials Sensitive Access Tokens"
metadata:
  actual:
    adr_id: cb991e0e-b919-4eb2-9a74-b5286ba22f17
    status: accepted
    candidate_group_id: f38733e0-f502-4a66-81d9-7b70c2173a8d
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Provider Integration: Outbound Provider Credentials Sensitive Access Tokens

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for all notification provider packages responsible for external alert delivery, transport dispatching, and integration modules consuming inbound notification payloads within the notification workspace.

### Rules

- **R-NOTIF-001** MUST: Outbound provider credentials and sensitive access tokens MUST be resolved from runtime environment configuration rather than hardcoded in source modules.

### Verify

```bash
# Discover and run the project's workspace-wide static type-checking script to confirm full contract adherence with @openstatus/notification-base.
# Discover and execute the test suite corresponding to notification delivery integrations to verify payload parsing and external dispatch behaviors.
```

**Accept when:**
- All notification provider packages compile without type errors against the imported base contract.
- Unit and integration tests for notification dispatch pass successfully, confirming valid schema parsing and external request dispatching.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon type incompatibility or unvalidated notification payload handling, and pull requests bypassing the shared base module contract are blocked until refactored.
</enforcement>