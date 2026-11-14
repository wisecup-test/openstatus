---
version: 2
name: cross-cutting-network-diagnostic-endpoints-regional-me-c855
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Network Diagnostic Endpoints Regional Metric Aggregations"
metadata:
  actual:
    adr_id: c855612f-efac-49c8-8cef-f447d4429f75
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

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Network Diagnostic Endpoints Regional Metric Aggregations

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/data/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all application modules, API route handlers, background tasks, and UI components handling geographic region identifiers, edge locations, or regional latency metrics.

### Rules

- **R-REGION-001** MUST: All network diagnostic endpoints, regional metric aggregations, and edge location interfaces MUST import geographic region definitions, schemas, and identifiers exclusively from @openstatus/regions.

### Verify

\`\`\`bash
# Discover the test script in the project dependency manifest and execute the test suite covering regional schema validation and probe routing.
# Discover the type inspection and static analysis script in the workspace configuration and execute it to verify all region imports resolve without error.
\`\`\`

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>