---
version: 2
name: cross-cutting-inspect-project-lock-artifact-resolve-e24b
description: "Adoption of @slack/web-api for Slack Platform Interactions: Inspect Project Lock Artifact Resolve Exact"
metadata:
  actual:
    adr_id: e24b74b6-728c-4d05-aeeb-3a88b2cc48da
    status: accepted
    candidate_group_id: e6951826-aafd-4c47-b9cc-b88232ab1112
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @slack/web-api for Slack Platform Interactions: Inspect Project Lock Artifact Resolve Exact

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLACK-001** MUST: Inspect the project lock artifact to resolve the exact installed version of @slack/web-api before implementing or modifying client invocation patterns.

### Verify

```bash
# Verify via typecheck, lint, and unit tests
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