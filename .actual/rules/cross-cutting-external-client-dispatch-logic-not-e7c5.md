---
version: 2
name: cross-cutting-external-client-dispatch-logic-not-e7c5
description: "Adoption of @openstatus/notification-base for Outbound Webhook Integration: External Client Dispatch Logic Not Bypass"
metadata:
  actual:
    adr_id: e7c5c48d-bb3e-44ab-92ba-d21481eff564
    status: accepted
    candidate_group_id: d10cb657-9b38-4b0f-86db-ef19fe31223b
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Outbound Webhook Integration: External Client Dispatch Logic Not Bypass

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST_NOT: External client dispatch logic MUST NOT bypass schema validation or send unvalidated raw event objects to remote webhook endpoints.

### Verify

```bash
# Discover workspace test execution script in project manifest and execute tests covering notification modules
# Discover project static type checking and linting tasks and run across notification package boundaries
# Inspect project dependency lock artifact to verify resolved package integrity and run automated contract verification suite
```

**Accept when:**
- All outbound notification payload schema validation tests pass without runtime parsing errors.
- HTTP dispatch mock tests confirm external webhook requests are correctly structured, serialized, and transmitted.
- Type checking and boundary contract verification suites execute with zero diagnostics across notification modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated test suites, CI pipelines, and pull request code reviews focusing on outbound client boundary contracts and schema parsing.
</enforcement>