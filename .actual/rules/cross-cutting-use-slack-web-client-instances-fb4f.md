---
version: 2
name: cross-cutting-use-slack-web-client-instances-fb4f
description: "Adoption of @slack/web-api for Slack Platform Interactions: Use Slack Web Client Instances Uniform"
metadata:
  actual:
    adr_id: fb4f7905-b0e4-4bcd-8cbf-adc142357492
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Use Slack Web Client Instances Uniform

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLACK-001** MUST: Use @slack/web-api client instances as the uniform interface for programmatic communication with Slack platform endpoints across all integration route handlers.

### Verify

```bash
find_project_root_and_execute_typecheck
find_project_root_and_execute_lint_check
find_project_root_and_execute_unit_tests
```

**Accept when:**
- Static analysis confirms all Slack platform calls route through the official client SDK rather than raw HTTP requests.
- All Slack interaction and view publishing handlers conform to typed client method signatures without compilation errors.
- Integration test suites pass with mocked client responses reflecting expected platform payload structures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>