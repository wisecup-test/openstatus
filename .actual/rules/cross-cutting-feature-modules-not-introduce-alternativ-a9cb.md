---
version: 2
name: cross-cutting-feature-modules-not-introduce-alternativ-a9cb
description: "Next.js Framework Adoption for Web Application Architecture: Feature Modules Not Introduce Alternative Application"
metadata:
  actual:
    adr_id: a9cb8882-3fd8-4e79-bbd9-184aa9a18254
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

# Next.js Framework Adoption for Web Application Architecture: Feature Modules Not Introduce Alternative Application

Scope: applies to `apps/dashboard/next.config.ts`, `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for web application modules and dashboard services requiring routing, layout composition, and web application lifecycle management.

### Rules

- **R-NEXT-001** MUST_NOT: Feature modules MUST NOT introduce alternative application routing or meta-framework runtimes within the application tier.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>