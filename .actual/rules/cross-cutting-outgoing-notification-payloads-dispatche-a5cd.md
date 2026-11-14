---
version: 2
name: cross-cutting-outgoing-notification-payloads-dispatche-a5cd
description: "Adoption of @openstatus/notification-base Core Library: Outgoing Notification Payloads Dispatched Across External"
metadata:
  actual:
    adr_id: a5cd0164-e094-4209-9270-392a60c9a0dc
    status: accepted
    candidate_group_id: 134a167d-ded5-4810-8ca0-70ed27e07461
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base Core Library: Outgoing Notification Payloads Dispatched Across External

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Outgoing notification payloads dispatched across external network boundaries MUST be validated against their target schema definition prior to transmission.

### Verify

```bash
# Execute integration tests for the notification handler using the repository's test runner
# Execute workspace type-checking to validate schema contract compliance
# Execute linting and static analysis to verify module boundary adherence
```

**Accept when:**
- All notification handler test suites execute and pass without schema validation or network delivery errors.
- Static type analysis confirms that all payload transformations conform to provider schema definitions.
- All external HTTP client calls transmit properly validated JSON payloads with correct content headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and code modifications must conform to shared base module architecture and schema parsing rules.
</enforcement>