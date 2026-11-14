---
version: 2
name: cross-cutting-tracking-calls-within-visual-status-96d5
description: "Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Tracking Calls Within Visual Status Components"
metadata:
  actual:
    adr_id: 96d53c49-4dd2-4768-93a0-28628be27de7
    status: accepted
    candidate_group_id: 49801d3f-23ee-49ad-a749-5c61c258693e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
---

# Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Tracking Calls Within Visual Status Components

Scope: applies to `apps/web/src/app/api/og/**`.

These rules are ALWAYS ACTIVE for dynamic Open Graph image generation route handlers and associated visual status indicator components.

### Rules

- **R-TRK-001** SHOULD: Tracking calls within visual status components SHOULD delegate telemetry payload formatting to shared utility helpers rather than embedding custom telemetry formatters directly in presentation components.

### Verify

```bash
# Discover the project dependency manifest and execute the designated typecheck script to verify module resolution for @openstatus/tracker.
# Execute the repository linting and test scripts discovered from the project configuration to confirm compliance across visual status components.
```

**Accept when:**
- All imports of @openstatus/tracker resolve cleanly during repository typecheck and build validation.
- Dynamic image generation route handlers and status components compile and pass all automated test suites without telemetry interface errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration typecheck, automated test pipeline execution, and peer code review enforce these rules.
</enforcement>