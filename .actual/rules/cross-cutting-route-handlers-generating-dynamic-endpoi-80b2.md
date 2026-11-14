---
version: 2
name: cross-cutting-route-handlers-generating-dynamic-endpoi-80b2
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Route Handlers Generating Dynamic Endpoints Metadata"
metadata:
  actual:
    adr_id: 80b2216f-0f62-4044-86da-5ed973adc9b3
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Route Handlers Generating Dynamic Endpoints Metadata

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered endpoints, route handlers, and metadata generation modules requiring access to persistent domain data.

### Rules

- **R-DB-001** SHOULD: Route handlers generating dynamic endpoints or metadata SHOULD resolve incoming request boundaries via runtime header utilities before querying @openstatus/db with tenant-scoped parameters.
- **R-DB-002** MANDATORY: Applications requiring database connectivity MUST import database client instances and canonical schema structures through the primary export entry points provided by @openstatus/db rather than deep-linking into unexported internal files or declaring duplicate local database entity schemas.

### Verify

```bash
# Discover the workspace dependency manifest and execute type-checking and linting across all consuming applications
workspace_pkg_manager=$(find . -maxdepth 2 -name "package.json" -print -quit)
if [ -f "pnpm-lock.yaml" ]; then
  pnpm check || pnpm tsc --noEmit
elif [ -f "bun.lockb" ] || [ -f "bun.lock" ]; then
  bun run check || bun tsc --noEmit
elif [ -f "yarn.lock" ]; then
  yarn check || yarn tsc --noEmit
else
  npm run check || npx tsc --noEmit
fi
```

**Accept when:**
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace-wide type verification and import boundary linting.
</enforcement>