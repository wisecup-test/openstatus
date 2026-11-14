---
version: 2
name: cross-cutting-routing-procedures-not-define-hoc-594d
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Routing Procedures Not Define Hoc Local"
metadata:
  actual:
    adr_id: 594de9a2-0669-4050-80d8-6a1e68a7e3d9
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Routing Procedures Not Define Hoc Local

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedures that accept, validate, or mutate synthetic monitor assertion configurations, and all execution handlers and checker dispatch endpoints that evaluate synthetic response assertions.

### Rules

- **R-ASSERT-001** SHOULD_NOT: API routing procedures SHOULD_NOT define ad-hoc or local schemas for synthetic check assertions outside of @openstatus/assertions.

### Verify

```bash
# Discover and run the workspace static analysis and type verification script from the repository manifest
# Discover and run the project test suite covering API router assertion validation and checker execution workflows
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checks and code reviews verify workspace dependency imports and schema usage, rejecting builds that introduce local assertion schema definitions.
</enforcement>