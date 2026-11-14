---
version: 2
name: cross-cutting-route-handlers-not-construct-bespoke-3f22
description: "Adoption of @openstatus/react for Status Badge Rendering: Route Handlers Not Construct Bespoke Inline"
metadata:
  actual:
    adr_id: 3f22b81d-6347-4c7a-83a7-696743421e6a
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Route Handlers Not Construct Bespoke Inline

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public status page badge route handlers and server-side endpoints rendering status indicator graphics from incoming query parameters.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: Route handlers MUST NOT construct bespoke, inline badge markup or visual templates independently of the adopted component library (@openstatus/react).
- **R-OPENSTATUS-002** MANDATORY: Execute lock-version grounding before writing code using versioned libraries (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).
- **R-OPENSTATUS-003** MANDATORY: Implement strict parameter sanitization and define default fallbacks for all input parameters (theme, size, variant) passed to badge components.

### Verify

```bash
# Find and execute the repository validation script defined in the project configuration
# to confirm type checking and build integrity for badge route handlers.
# Run the test suite discovered from the package configuration to verify badge route handler responses and parameter resolution.
npm test
```

**Accept when:**
- The repository test suite passes for all status badge route endpoints without regressions.
- All badge rendering routes resolve components exclusively from @openstatus/react without local styling duplication.
- Dependency resolution confirms that the installed version of @openstatus/react matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes touching status badge route handlers are subject to automated dependency/type verification, peer code reviews, and strict blocking of inline markup introductions.
</enforcement>