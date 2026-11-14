---
version: 2
name: cross-cutting-notification-dispatch-integration-module-c6b5
description: "Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Notification Dispatch Integration Modules Consume Shared"
metadata:
  actual:
    adr_id: c6b5a477-d1e3-47ff-a8f1-885b36339fa0
    status: accepted
    candidate_group_id: 9bd500d7-caf0-4fe0-ae1e-44d97d6b6b6c
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Notification Dispatch Integration Modules Consume Shared

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for outbound notification handler modules that adapt internal notification payloads into external client requests.

### Rules

- **R-NOTIF-001** MUST: Notification dispatch integration modules MUST consume the shared @openstatus/notification-base foundation to define notification handling structures and contract boundaries.

### Verify

```bash
# Discover and run the project's static analysis, type-checking, and test suite commands from the build configuration
# Example typical commands (derived from repository configuration):
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- Incoming notification payloads pass schema validation before triggering network calls.
- Outbound HTTP POST requests successfully serialize JSON content to target webhook endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated build pipeline checks and peer code review.
</enforcement>