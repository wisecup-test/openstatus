---
version: 2
name: cross-cutting-outbound-http-webhook-transmissions-conf-c7c3
description: "Adoption of @openstatus/notification-base Core Library: Outbound Http Webhook Transmissions Configure Standardized"
metadata:
  actual:
    adr_id: c7c3d12d-9198-4e48-837d-16d9ee8246d7
    status: accepted
    candidate_group_id: 134a167d-ded5-4810-8ca0-70ed27e07461
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base Core Library: Outbound Http Webhook Transmissions Configure Standardized

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** SHOULD: Outbound HTTP webhook transmissions SHOULD configure standardized JSON content headers and transmit stringified event data across external client boundaries.

### Verify

```bash
# Discover and execute integration tests for the notification handler via repository build manifest
# Discover and execute workspace type-checking to validate schema contract compliance
# Discover and execute linting and static analysis to verify module boundary adherence
```

**Accept when:**
- All notification handler test suites execute and pass without schema validation or network delivery errors.
- Static type analysis confirms that all payload transformations conform to provider schema definitions.
- All external HTTP client calls transmit properly validated JSON payloads with correct content headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated CI pipelines and mandatory peer code reviews.
</enforcement>