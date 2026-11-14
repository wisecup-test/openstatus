---
version: 2
name: cross-cutting-channel-specific-message-block-builders-9581
description: "Adoption of @openstatus/notification-base for Notification Channel Modules: Channel Specific Message Block Builders Webhook"
metadata:
  actual:
    adr_id: 9581560d-2d69-413f-a499-b65e0a2da72e
    status: accepted
    candidate_group_id: d4bf6f6d-dbc1-4ba6-ab0c-83430fda99ff
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Channel Modules: Channel Specific Message Block Builders Webhook

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, channel providers, message block formatters, and dispatch handlers within the notification subsystem.

### Rules

- **R-NOTIF-001** SHOULD: Channel-specific message block builders and webhook dispatchers SHOULD consume base notification types from @openstatus/notification-base while encapsulating vendor-specific payload formatting within their respective packages.

### Verify

```bash
# Discover workspace dependency graph and verify notification channel packages declare and resolve @openstatus/notification-base
# Execute repository static analysis and typecheck verification routines
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all notification channel packages importing @openstatus/notification-base.
- Dependency graph inspection confirms all notification modules resolve @openstatus/notification-base consistently according to the repository resolution artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated workspace dependency boundary checks, static analysis, and mandatory peer code review enforce these rules; violations block PRs in continuous integration.
</enforcement>