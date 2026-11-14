---
version: 2
name: cross-cutting-before-implementing-refactoring-features-b517
description: "Next.js Framework Adoption for Web Application Architecture: Before Implementing Refactoring Features Next Developers"
metadata:
  actual:
    adr_id: b5178d27-5bd0-4b7a-a8f4-efcebd9210b4
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

# Next.js Framework Adoption for Web Application Architecture: Before Implementing Refactoring Features Next Developers

Scope: applies to `apps/dashboard/next.config.ts`, `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for web application modules and dashboard services requiring routing, layout composition, and web application lifecycle management.

### Rules

- **R-NXT-001** MUST: Before implementing or refactoring features using Next.js APIs, developers MUST discover the ecosystem lock file and verify the exact resolved framework version against authoritative version documentation.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>