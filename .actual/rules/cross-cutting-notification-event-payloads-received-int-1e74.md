---
version: 2
name: cross-cutting-notification-event-payloads-received-int-1e74
description: "Adoption of @openstatus/notification-base Core Library: Notification Event Payloads Received Integration Handlers"
metadata:
  actual:
    adr_id: 1e74496c-3bff-4015-83ec-7ce17ecc06f8
    status: accepted
    candidate_group_id: 134a167d-ded5-4810-8ca0-70ed27e07461
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base Core Library: Notification Event Payloads Received Integration Handlers

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification event payloads received by integration handlers MUST undergo schema validation using parse operations prior to extracting monitor state and status codes.

### Verify

```bash
# Discover and execute the test runner command from the repository build manifest for the notification handler integration tests
# Discover and execute the workspace type-checking command from the project configuration to validate schema contract compliance
# Discover and execute the linting and static analysis command from the root configuration to verify module boundary adherence
```

**Accept when:**
- All notification handler test suites execute and pass without schema validation or network delivery errors.
- Static type analysis confirms that all payload transformations conform to provider schema definitions.
- All external HTTP client calls transmit properly validated JSON payloads with correct content headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration pipelines and mandatory peer code reviews is strictly enforced; pull requests failing schema validation tests or omitting base module contracts are blocked from merging.
</enforcement>