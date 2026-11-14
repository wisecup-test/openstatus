---
version: 2
name: cross-cutting-telemetry-integration-image-generation-e-01ba
description: "Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Telemetry Integration Image Generation Endpoints Handle"
metadata:
  actual:
    adr_id: 01baabab-efbb-4eee-972b-3d371a7cc364
    status: accepted
    candidate_group_id: 49801d3f-23ee-49ad-a749-5c61c258693e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
---

# Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Telemetry Integration Image Generation Endpoints Handle

Scope: applies to `apps/web/src/app/api/og/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Telemetry integration in image generation endpoints MUST handle tracking failures gracefully without interrupting image response rendering.

### Verify

```bash
# Discover dependency manifest and run typecheck
# (e.g., npm run typecheck or yarn typecheck / pnpm typecheck)

# Run repository linting and test suites
# (e.g., npm test or yarn test / pnpm test)
```

**Accept when:**
- All imports of @openstatus/tracker resolve cleanly during repository typecheck and build validation.
- Dynamic image generation route handlers and status components compile and pass all automated test suites without telemetry interface errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>