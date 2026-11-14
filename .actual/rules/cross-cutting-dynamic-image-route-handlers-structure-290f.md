---
version: 2
name: cross-cutting-dynamic-image-route-handlers-structure-290f
description: "Adoption of next/og for Dynamic Open Graph Image Generation: Dynamic Image Route Handlers Structure Graphic"
metadata:
  actual:
    adr_id: 290f26fe-0266-4938-a342-a5d084f033a4
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

# Adoption of next/og for Dynamic Open Graph Image Generation: Dynamic Image Route Handlers Structure Graphic

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OG-001** SHOULD: Dynamic image route handlers SHOULD structure graphic presentations using shared modular layout components rather than defining inline bespoke graphic structures.

### Verify

```bash
# Discover workspace test execution script and run endpoint integration tests targeting image generation routes
npm test
# Run type-checking and validation across image endpoint modules
npm run typecheck
```

**Accept when:**
- All dynamic image route endpoints resolve next/og imports and compile without type or layout errors.
- Automated test suites verify image endpoints return appropriate content-type headers and handle missing query parameters gracefully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking, automated route integration tests, and peer code review verification are mandatory.
</enforcement>