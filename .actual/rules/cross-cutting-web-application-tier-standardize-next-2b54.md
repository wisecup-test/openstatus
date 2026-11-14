---
version: 2
name: cross-cutting-web-application-tier-standardize-next-2b54
description: "Next.js Framework Adoption for Web Application Architecture: Web Application Tier Standardize Next Primary"
metadata:
  actual:
    adr_id: 2b548d28-589a-40c4-8877-046f685d6bd9
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

# Next.js Framework Adoption for Web Application Architecture: Web Application Tier Standardize Next Primary

Scope: applies to `apps/dashboard/next.config.ts`, `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all web application modules and dashboard services requiring routing, layout composition, and web application lifecycle management.

### Rules

- **R-NEXT-001** MUST: The web application tier MUST standardize on Next.js as the primary core application framework for routing, layout hierarchy, and metadata generation.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and code reviews will block code that violates route hierarchies or framework configurations.
</enforcement>