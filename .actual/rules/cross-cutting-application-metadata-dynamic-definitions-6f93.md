---
version: 2
name: cross-cutting-application-metadata-dynamic-definitions-6f93
description: "Next.js Framework Adoption for Web Application Architecture: Application Metadata Dynamic Definitions Crawler Directives"
metadata:
  actual:
    adr_id: 6f9372ee-9c25-477f-8f7e-a7323ab73a81
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

# Next.js Framework Adoption for Web Application Architecture: Application Metadata Dynamic Definitions Crawler Directives

Scope: applies to `apps/dashboard/next.config.ts`, `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for web application modules and dashboard services requiring routing, layout composition, and web application lifecycle management.

### Rules

- **R-NEXT-001** SHOULD: Application metadata, dynamic metadata definitions, and crawler directives SHOULD use native framework metadata and robots handlers rather than ad-hoc custom routing middleware.

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
Verification via automated continuous integration and code review is mandatory. CI failures block merging of code with broken route hierarchies or invalid framework configurations.
</enforcement>