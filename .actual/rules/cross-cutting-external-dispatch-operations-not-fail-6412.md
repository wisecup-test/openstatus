---
version: 2
name: cross-cutting-external-dispatch-operations-not-fail-6412
description: "Adoption of @openstatus/notification-base for Notification Dispatch Handlers: External Dispatch Operations Not Fail Silently"
metadata:
  actual:
    adr_id: 641275c0-733d-471a-aeff-758d82106b96
    status: accepted
    candidate_group_id: 9bd500d7-caf0-4fe0-ae1e-44d97d6b6b6c
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Handlers: External Dispatch Operations Not Fail Silently

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for outbound notification handler modules that adapt internal notification payloads into external client requests.

### Rules

- **R-DISPATCH-001** SHOULD: External dispatch operations MUST NOT fail silently and SHOULD log execution errors with descriptive context when network requests or schema parsing fails.

### Verify

```bash
# Discover and run the project's static analysis and type-checking scripts
npm run typecheck || pnpm typecheck || yarn typecheck

# Discover and execute the project's unit and integration test suite
npm test || pnpm test || yarn test
```

**Accept when:**
- Incoming notification payloads pass schema validation before triggering network calls.
- Outbound HTTP POST requests successfully serialize JSON content to target webhook endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>