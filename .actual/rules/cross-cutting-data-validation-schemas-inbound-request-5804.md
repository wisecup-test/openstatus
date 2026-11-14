---
version: 2
name: cross-cutting-data-validation-schemas-inbound-request-5804
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Data Validation Schemas Inbound Request Payloads"
metadata:
  actual:
    adr_id: 58041da5-89dc-4e35-aaed-1c039bc06387
    status: accepted
    candidate_group_id: 903339f8-5d24-42ec-828d-2745bb17f92b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/data/**
      - src/lib/checker/**
---

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Data Validation Schemas Inbound Request Payloads

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/data/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all application modules, API route handlers, background tasks, and UI components handling geographic region identifiers, edge locations, or regional latency metrics.

### Rules

- **R-OPENSTATUS-001** MUST: Data validation schemas for inbound request payloads and probe execution targets MUST incorporate region validation schemas exported by @openstatus/regions.

### Verify

```bash
# Discover the test script in the project dependency manifest and execute the test suite covering regional schema validation and probe routing.
pnpm test
# Discover the type inspection and static analysis script in the workspace configuration and execute it to verify all region imports resolve without error.
pnpm check
```

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated review gates and static analysis checks in the CI pipeline enforce these rules.
</enforcement>