---
version: 2
name: cross-cutting-component-rendering-logic-within-image-9e41
description: "Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Component Rendering Logic Within Image Generation"
metadata:
  actual:
    adr_id: 9e411f78-d9f5-4ffc-b7a7-dd11342d7d13
    status: accepted
    candidate_group_id: 49801d3f-23ee-49ad-a749-5c61c258693e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
---

# Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Component Rendering Logic Within Image Generation

Scope: applies to `apps/web/src/app/api/og/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OGT-001** MUST_NOT: Component rendering logic within the image generation pipeline MUST_NOT instantiate ad-hoc or secondary telemetry trackers outside the approved @openstatus/tracker package interface.

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