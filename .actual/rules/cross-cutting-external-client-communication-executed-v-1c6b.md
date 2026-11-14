---
version: 2
name: cross-cutting-external-client-communication-executed-v-1c6b
description: "Adoption of @openstatus/notification-base for Notification Dispatch Handlers: External Client Communication Executed Via Fetch"
metadata:
  actual:
    adr_id: 1c6be3a7-2770-4888-8450-289eadd324ff
    status: accepted
    candidate_group_id: 9bd500d7-caf0-4fe0-ae1e-44d97d6b6b6c
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Handlers: External Client Communication Executed Via Fetch

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: External client communication MUST be executed via fetch using HTTP POST with an explicit application/json content-type header and a serialized request body.

### Verify

```bash
# Discover and run the project's static analysis and type-checking scripts from the build configuration
# Discover and execute the project's unit and integration test suite
```

**Accept when:**
- Incoming notification payloads pass schema validation before triggering network calls.
- Outbound HTTP POST requests successfully serialize JSON content to target webhook endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification via static analysis, type-checking, and test execution is mandatory.
</enforcement>