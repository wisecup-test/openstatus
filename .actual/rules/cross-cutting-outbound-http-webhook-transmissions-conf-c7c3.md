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

These rules are ALWAYS ACTIVE for all notification provider integration packages and modules transforming internal monitor status events into external third-party webhook payloads.

### Rules

- **R-NOTIF-001** SHOULD: Outbound HTTP webhook transmissions configure standardized JSON content headers and transmit stringified event data across external client boundaries.

### Verify

```bash
# Discover and run integration tests for the notification handler from the repository build manifest
# Discover and run workspace type-checking from project configuration to validate schema contract compliance
# Discover and run linting/static analysis from root configuration to verify module boundary adherence
```

**Accept when:**
- All notification handler test suites execute and pass without schema validation or network delivery errors.
- Static type analysis confirms that all payload transformations conform to provider schema definitions.
- All external HTTP client calls transmit properly validated JSON payloads with correct content headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI pipelines and peer code reviews enforce compliance; pull requests failing schema validation or omitting base module contracts are blocked.
</enforcement>