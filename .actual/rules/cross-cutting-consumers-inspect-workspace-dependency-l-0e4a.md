---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-l-0e4a
description: "Adoption of @openstatus/notification-base for Outbound Webhook Integration: Consumers Inspect Workspace Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 0e4aa80e-cebc-4d66-88db-53b51f58a1b1
    status: accepted
    candidate_group_id: d10cb657-9b38-4b0f-86db-ef19fe31223b
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Outbound Webhook Integration: Consumers Inspect Workspace Dependency Lock Artifact

Scope: applies to `packages/notifications/grafana-oncall/src/**`.

These rules are ALWAYS ACTIVE for packages and modules responsible for formatting and dispatching notification events to external incident management webhooks, and outbound client boundaries integrating external alert delivery services.

### Rules

- **R-ADR-001** MUST: Consumers MUST inspect the workspace dependency lock artifact to identify the exact resolved version of @openstatus/notification-base and its peer dependencies before implementing or modifying notification adapters.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated unit and integration test suites, pull request code reviews, and build/test step enforcement.
</enforcement>