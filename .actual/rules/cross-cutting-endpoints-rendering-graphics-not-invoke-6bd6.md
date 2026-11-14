---
version: 2
name: cross-cutting-endpoints-rendering-graphics-not-invoke-6bd6
description: "Adoption of next/og for Dynamic Open Graph Image Generation: Endpoints Rendering Graphics Not Invoke Uncurated"
metadata:
  actual:
    adr_id: 6bd682e2-bca4-4282-b85d-61d4a139be33
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

# Adoption of next/og for Dynamic Open Graph Image Generation: Endpoints Rendering Graphics Not Invoke Uncurated

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OP-001** MUST_NOT: Endpoints rendering graphics MUST NOT invoke uncurated third-party rendering services or untrusted child processes to generate social preview images.

### Verify

```bash
# Discover and run the workspace test execution script targeting image generation routes
# Identify workspace type-checking and linting configuration and run validation across image endpoint modules
```

**Accept when:**
- All dynamic image route endpoints resolve next/og imports and compile without type or layout errors.
- Automated test suites verify image endpoints return appropriate content-type headers and handle missing query parameters gracefully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified by continuous integration type checking, automated route integration tests, and peer code review.
</enforcement>