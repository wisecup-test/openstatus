---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-m-a6b8
description: "Adoption of @openstatus/theme-store for Theme Schema Definition: Consumers Inspect Workspace Dependency Manifests Resolution"
metadata:
  actual:
    adr_id: a6b8d393-f1b6-42d0-8d2a-6997b8fd34eb
    status: accepted
    candidate_group_id: 953354fc-1228-4815-8dc1-ee03dbe3f2ef
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/pages/**
---

# Adoption of @openstatus/theme-store for Theme Schema Definition: Consumers Inspect Workspace Dependency Manifests Resolution

Scope: applies to `packages/db/src/schema/pages/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-THEME-001** MUST: Consumers MUST inspect the workspace dependency manifests and resolution artifacts to ground the exact resolved module contract before referencing exports from @openstatus/theme-store.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify that theme references resolve to the shared module.
npm run lint || pnpm lint || bun run lint

# Discover and execute test suites targeting database and validation schemas to confirm proper theme schema enforcement.
npm test || pnpm test || bun test
```

**Accept when:**
- All schema and validation declarations requiring theme configuration import directly from @openstatus/theme-store.
- Repository static analysis and dependency graph audits confirm zero duplicate theme schema definitions across packages.
- Validation and schema tests verify that invalid theme variable structures are rejected at runtime.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated module boundary linting and dependency tree verification will block violations.
</enforcement>