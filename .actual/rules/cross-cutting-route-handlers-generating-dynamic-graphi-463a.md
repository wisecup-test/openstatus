---
version: 2
name: cross-cutting-route-handlers-generating-dynamic-graphi-463a
description: "Adoption of next/og for Dynamic Open Graph Image Generation: Route Handlers Generating Dynamic Graphics Extract"
metadata:
  actual:
    adr_id: 463afee9-538d-49f7-961a-bc3bcf0ecc74
    status: accepted
    candidate_group_id: 6c49511a-9093-466c-988d-7593d1c959e4
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
      - src/lib/checker/**
---

# Adoption of next/og for Dynamic Open Graph Image Generation: Route Handlers Generating Dynamic Graphics Extract

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for API endpoints and server handlers dedicated to rendering dynamic Open Graph and social preview image assets, as well as modules providing visual layout components specifically designed for dynamic metadata graphics.

### Rules

- **R-OG-001** MUST: Route handlers generating dynamic graphics MUST extract runtime parameters through request search parameter accessors and validate input parameters before passing them to the image template.

### Verify

```bash
# Discover workspace test execution script in project manifest and run endpoint integration tests
# Identify workspace type-checking and linting configuration and run validation across image endpoint modules
```

**Accept when:**
- All dynamic image route endpoints resolve next/og imports and compile without type or layout errors.
- Automated test suites verify image endpoints return appropriate content-type headers and handle missing query parameters gracefully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking, automated route integration tests, and peer code reviews enforce compliance.
</enforcement>