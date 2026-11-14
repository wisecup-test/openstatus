---
version: 2
name: cross-cutting-modules-not-accept-unvalidated-external-2d19
description: "Zod Schema Definition and Runtime Validation Library Adoption: Modules Not Accept Unvalidated External Inputs"
metadata:
  actual:
    adr_id: 2d196e3f-9aa3-45a5-9ed9-2d722c578ce0
    status: accepted
    candidate_group_id: c1f28a38-ae70-4493-874a-5738669cad24
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation Library Adoption: Modules Not Accept Unvalidated External Inputs

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all boundary entry points, HTTP routes, RPC adapters, external integration webhooks, tool registries, and environment configuration loaders.

### Rules

- **R-ZOD-001** MUST_NOT: Modules MUST_NOT accept unvalidated external inputs or use unchecked type assertions to bypass runtime schema parsing at network or protocol boundaries.

### Verify

```bash
# Discover and execute the project test runner script defined in the dependency manifest
# Discover and execute the static type verification script declared in workspace configuration
# Discover and execute the project linter and format validation scripts
```

**Accept when:**
- All schema validation test suites pass successfully across boundary adapters and route handlers.
- Static type analysis passes with zero diagnostic errors across all modules defining or consuming schema-inferred types.
- Runtime input parsing reliably rejects malformed boundary payloads and permits valid payloads across all integration entry points.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation results in continuous integration build failures and pull request rejections.
</enforcement>