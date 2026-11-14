---
version: 2
name: cross-cutting-handle-platform-specific-error-responses-d22c
description: "Adoption of @slack/web-api for Slack Platform Interactions: Handle Platform Specific Error Responses Systematically"
metadata:
  actual:
    adr_id: d22c4677-6654-4ee3-ab2b-8b2cba463b5e
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Handle Platform Specific Error Responses Systematically

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server-side route modules handling incoming Slack events, slash commands, interactive component callbacks, home tab views, and internal services that initiate outbound messaging or view updates to the Slack platform.

### Rules

- **R-SLACK-001** SHOULD: Handle platform-specific error responses systematically, logging contextual metadata and updating corresponding interface surfaces.
- **R-SLACK-002** MANDATORY: Rely on lock artifact resolution and run contract verification tests against mocked SDK responses during CI runs.
- **R-SLACK-003** MANDATORY: Configure client instances using workspace environment secrets resolved at runtime rather than hardcoding integration credentials.
- **R-SLACK-004** MANDATORY: Structure message update calls to catch platform error codes and trigger appropriate fallback UI states or cleanup routines.
- **R-SLACK-005** MANDATORY: Structure modal and home tab view payloads using dedicated view building functions before passing them to the publish API.

### Verify

```bash
# Execute project typechecking, linting, and unit tests to verify Slack platform integration conformance
find_project_root_and_execute_typecheck
find_project_root_and_execute_lint_check
find_project_root_and_execute_unit_tests
```

**Accept when:**
- Static analysis confirms all Slack platform calls route through the official client SDK rather than raw HTTP requests.
- All Slack interaction and view publishing handlers conform to typed client method signatures without compilation errors.
- Integration test suites pass with mocked client responses reflecting expected platform payload structures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All Slack platform integration code must strictly use the official client SDK, handle errors systematically, and pass all static analysis, linting, and test gates.
</enforcement>