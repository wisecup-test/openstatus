---
version: 2
name: cross-cutting-outbound-http-dispatches-serialize-valid-380e
description: "Adoption of @openstatus/notification-base for Outbound Webhook Integration: Outbound Http Dispatches Serialize Validated Payload"
metadata:
  actual:
    adr_id: 380ecebc-bee6-4e65-9ba9-8b6936e1e9d9
    status: accepted
    candidate_group_id: d10cb657-9b38-4b0f-86db-ef19fe31223b
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Outbound Webhook Integration: Outbound Http Dispatches Serialize Validated Payload

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for packages and modules responsible for formatting and dispatching notification events to external incident management webhooks, and outbound client boundaries integrating external alert delivery services.

### Rules

- **R-ON-001** MUST: Outbound HTTP dispatches MUST serialize validated payload objects to JSON and submit them via POST requests containing an explicit application/json content-type header.

### Verify

```bash
# Discover the workspace test execution script in the project manifest and execute the test suite covering notification integration modules.
# Discover the project static type checking and linting tasks and run them across all notification package boundaries.
# Inspect the project dependency lock artifact to verify resolved package integrity and run the automated contract verification suite.
```

**Accept when:**
- All outbound notification payload schema validation tests pass without runtime parsing errors.
- HTTP dispatch mock tests confirm external webhook requests are correctly structured, serialized, and transmitted.
- Type checking and boundary contract verification suites execute with zero diagnostics across notification modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites, pull request code reviews, and build/test failures for unvalidated outbound payload dispatches.
</enforcement>