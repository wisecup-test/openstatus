---
version: 2
name: cross-cutting-request-tracking-status-telemetry-within-1dc3
description: "Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Request Tracking Status Telemetry Within Open"
metadata:
  actual:
    adr_id: 1dc37264-9729-41d8-b8ed-bbc6e360f973
    status: accepted
    candidate_group_id: 49801d3f-23ee-49ad-a749-5c61c258693e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
---

# Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Request Tracking Status Telemetry Within Open

Scope: applies to `apps/web/src/app/api/og/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: All request tracking and status telemetry within Open Graph image generation routes and visual status check components MUST use @openstatus/tracker.

### Verify

```bash
# Discover the project dependency manifest and execute the designated typecheck script to verify module resolution for @openstatus/tracker.
# Execute the repository linting and test scripts discovered from the project configuration to confirm compliance across visual status components.
```

**Accept when:**
- All imports of @openstatus/tracker resolve cleanly during repository typecheck and build validation.
- Dynamic image generation route handlers and status components compile and pass all automated test suites without telemetry interface errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>