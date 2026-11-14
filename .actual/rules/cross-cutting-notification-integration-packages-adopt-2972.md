---
version: 2
name: cross-cutting-notification-integration-packages-adopt-2972
description: "Adoption of @openstatus/notification-base for Outbound Webhook Integration: Notification Integration Packages Adopt Openstatus Base"
metadata:
  actual:
    adr_id: 29722113-a55d-407d-962f-9cbf472da035
    status: accepted
    candidate_group_id: d10cb657-9b38-4b0f-86db-ef19fe31223b
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Outbound Webhook Integration: Notification Integration Packages Adopt Openstatus Base

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification integration packages MUST adopt @openstatus/notification-base and schema-driven payload validation prior to external dispatch, ensuring all outbound alert data conforms to verified contract schemas.

### Verify

```bash
# Discover and execute the workspace test execution script covering notification integration modules
# Run project static type checking and linting tasks across notification package boundaries
# Inspect project dependency lock artifact and run automated contract verification suite
```

**Accept when:**
- All outbound notification payload schema validation tests pass without runtime parsing errors.
- HTTP dispatch mock tests confirm external webhook requests are correctly structured, serialized, and transmitted.
- Type checking and boundary contract verification suites execute with zero diagnostics across notification modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated unit and integration test suites execute in CI pipelines, and pull request code reviews enforce outbound client boundary contracts and schema parsing.
</enforcement>