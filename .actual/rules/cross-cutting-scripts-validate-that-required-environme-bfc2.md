---
version: 2
name: cross-cutting-scripts-validate-that-required-environme-bfc2
description: "Direct Process Environment Variable Access via Process Module: Scripts Validate That Required Environment Properties"
metadata:
  actual:
    adr_id: bfc24ffe-8275-462f-a477-cecf2e7356a6
    status: accepted
    candidate_group_id: 641a54a0-56d3-41a2-a284-dcbd11067dc6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Direct Process Environment Variable Access via Process Module: Scripts Validate That Required Environment Properties

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build automation and transformation scripts requiring platform deployment context, and local development execution where ambient runtime variables dictate asset resolution.

### Rules

- **R-ENV-001** MUST: Scripts MUST validate that required environment properties are defined before consuming their values during execution.
- **R-ENV-002** MUST: Verify that all accessed environment variables are documented in the repository environment documentation.
- **R-ENV-003** MUST: Ensure sensitive platform variables are not logged or exposed in transformed output artifacts.

### Verify

```bash
# Discover and run repository verification scripts to confirm environment configuration parsing complies with project standards
# Execute repository linting suites to ensure process environment access remains restricted to authorized utility scripts
```

**Accept when:**
- Repository verification scripts pass without reporting unauthorized process environment access.
- Utility scripts successfully parse required platform environment variables and exit cleanly.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via static analysis checks and peer code reviews restricting direct environment access to script scopes.
</enforcement>