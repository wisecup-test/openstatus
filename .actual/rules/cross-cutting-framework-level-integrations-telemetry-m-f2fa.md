---
version: 2
name: cross-cutting-framework-level-integrations-telemetry-m-f2fa
description: "Next.js Framework Adoption for Web Application Architecture: Framework Level Integrations Telemetry Monitoring Query"
metadata:
  actual:
    adr_id: f2fabf60-d114-4392-ab88-492c2dfd9d55
    status: accepted
    candidate_group_id: 30081942-db34-4cf4-a59f-fe6c9650c35d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - next.config.ts
      - src/app/**
---

# Next.js Framework Adoption for Web Application Architecture: Framework Level Integrations Telemetry Monitoring Query

Scope: applies to `apps/dashboard/next.config.ts`, `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FRAMEWORK-001** SHOULD: Framework-level integrations for telemetry, monitoring, or query state parsing SHOULD utilize framework-compatible plugins and integrations that attach directly to the framework configuration and page lifecycle.

### Verify

```bash
# Discover and run the project type check script defined in the repository manifest.
# Discover and execute the project build script to verify framework compilation and route integrity.
# Discover and execute the project linting and automated test suite to validate route compliance.
```

**Accept when:**
- The project build script executes successfully with zero compilation or route resolution errors.
- Static type analysis and framework validation pass across all route segments, layouts, and metadata definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All route hierarchies, framework configurations, and integrated plugins must comply with framework-native conventions.
</enforcement>