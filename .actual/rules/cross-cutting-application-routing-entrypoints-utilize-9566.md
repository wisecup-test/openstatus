---
version: 2
name: cross-cutting-application-routing-entrypoints-utilize-9566
description: "Next.js Framework Adoption for Web Application Architecture: Application Routing Entrypoints Utilize Native Framework"
metadata:
  actual:
    adr_id: 9566337f-a366-4f44-833a-b81dc5ce8c7a
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

# Next.js Framework Adoption for Web Application Architecture: Application Routing Entrypoints Utilize Native Framework

Scope: applies to `apps/dashboard/next.config.ts`, `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all web application modules and dashboard services requiring routing, layout composition, and web application lifecycle management.

### Rules

- **R-NEXT-001** MUST: Application routing and entrypoints MUST utilize native framework layout and routing conventions for structuring page hierarchies and route segment boundaries.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration and code review.
</enforcement>