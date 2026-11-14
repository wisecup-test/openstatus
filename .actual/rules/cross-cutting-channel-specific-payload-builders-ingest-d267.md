---
version: 2
name: cross-cutting-channel-specific-payload-builders-ingest-d267
description: "Adoption of @openstatus/notification-base for Notification Packages: Channel Specific Payload Builders Ingest Common"
metadata:
  actual:
    adr_id: d26792e1-241f-4f8b-bfb4-f0eb0c6337c4
    status: accepted
    candidate_group_id: 76fa8e0d-c217-4490-ba72-6b02bcf6281e
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Packages: Channel Specific Payload Builders Ingest Common

Scope: applies to `packages/notifications/ms-teams/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** SHOULD: Channel-specific payload builders SHOULD ingest the common notification context provided by @openstatus/notification-base and map it to provider-specific attachment and message schemas.

### Verify

```bash
# Discover and execute the repository dependency boundary validation check to verify that notification channel modules consume @openstatus/notification-base.
# Discover and execute the repository build and type verification suite to ensure contract alignment with @openstatus/notification-base.
```

**Accept when:**
- All notification integration modules consume shared notification types, interfaces, or context structures exclusively through @openstatus/notification-base.
- Static analysis and build checks confirm that notification channel adapters do not duplicate baseline notification contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>