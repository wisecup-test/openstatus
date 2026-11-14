---
version: 2
name: cross-cutting-badge-route-handlers-map-sanitized-d938
description: "Adoption of @openstatus/react for Status Badge Rendering: Badge Route Handlers Map Sanitized Url"
metadata:
  actual:
    adr_id: d938ae14-4c77-4c1a-b960-1d11582197bb
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Badge Route Handlers Map Sanitized Url

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public status page badge route handlers responsible for generating dynamic badge responses and server-side endpoints rendering status indicator graphics from incoming query parameters.

### Rules

- **R-BADGE-001** SHOULD: Badge route handlers SHOULD map sanitized URL search parameters directly to component properties supported by @openstatus/react.

### Verify

```bash
# Find and execute repository validation script and test suite
npm run test
```

**Accept when:**
- The repository test suite passes for all status badge route endpoints without regressions.
- All badge rendering routes resolve components exclusively from @openstatus/react without local styling duplication.
- Dependency resolution confirms that the installed version of @openstatus/react matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency and type verification during continuous integration checks and peer code reviews apply.
</enforcement>