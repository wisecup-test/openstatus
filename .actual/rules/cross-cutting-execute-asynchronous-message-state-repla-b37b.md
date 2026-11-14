---
version: 2
name: cross-cutting-execute-asynchronous-message-state-repla-b37b
description: "Adoption of @slack/web-api for Slack Platform Interactions: Execute Asynchronous Message State Replacements Conversational"
metadata:
  actual:
    adr_id: b37b29cb-bdf1-48e8-9a9b-f21b204ad88b
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Execute Asynchronous Message State Replacements Conversational

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLACK-001** MUST: Execute asynchronous message state replacements and conversational feedback updates via chat.update client methods.

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