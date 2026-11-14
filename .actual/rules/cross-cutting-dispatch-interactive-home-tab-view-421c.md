---
version: 2
name: cross-cutting-dispatch-interactive-home-tab-view-421c
description: "Adoption of @slack/web-api for Slack Platform Interactions: Dispatch Interactive Home Tab View Rendering"
metadata:
  actual:
    adr_id: 421c3134-f3a8-47e7-9239-e4df8bd48ab9
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Dispatch Interactive Home Tab View Rendering

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server-side route modules responsible for handling incoming Slack events, slash commands, interactive component callbacks, and home tab views, as well as internal services that initiate outbound messaging or view updates to the Slack platform.

### Rules

- **R-SLACK-001** MUST: Dispatch interactive home tab view rendering exclusively via views.publish client methods.

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