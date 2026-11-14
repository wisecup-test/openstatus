---
version: 2
name: cross-cutting-before-implementing-updating-tracking-in-9d8f
description: "Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Before Implementing Updating Tracking Integrations Consumers"
metadata:
  actual:
    adr_id: 9d8ffb1f-e5f9-4496-ada6-c456144f337c
    status: accepted
    candidate_group_id: 49801d3f-23ee-49ad-a749-5c61c258693e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
---

# Adoption of @openstatus/tracker for Open Graph Image Telemetry and Status Tracking: Before Implementing Updating Tracking Integrations Consumers

Scope: applies to `apps/web/src/app/api/og/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRACK-001** MUST: Before implementing or updating tracking integrations, consumers MUST inspect the project dependency manifest and resolved lock artifact to verify the exact installed library version.

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