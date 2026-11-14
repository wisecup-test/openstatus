---
version: 2
name: cross-cutting-validate-incoming-request-payloads-befor-906e
description: "Adoption of @slack/web-api for Slack Platform Interactions: Validate Incoming Request Payloads Before Passing"
metadata:
  actual:
    adr_id: 906ec104-dba5-45c8-97c6-b5c389e3afe4
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Validate Incoming Request Payloads Before Passing

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server-side route modules handling incoming Slack events, slash commands, interactive component callbacks, and home tab views, as well as internal services that initiate outbound messaging or view updates to the Slack platform.

### Rules

- **R-SLK-001** MUST: Validate incoming request payloads before passing extracted identifiers and parameters into Slack Web API client calls.

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