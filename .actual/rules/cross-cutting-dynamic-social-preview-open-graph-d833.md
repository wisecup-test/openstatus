---
version: 2
name: cross-cutting-dynamic-social-preview-open-graph-d833
description: "Adoption of next/og for Dynamic Open Graph Image Generation: Dynamic Social Preview Open Graph Card"
metadata:
  actual:
    adr_id: d833833d-ac1f-4164-b7f8-fbd5534fd290
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

# Adoption of next/og for Dynamic Open Graph Image Generation: Dynamic Social Preview Open Graph Card

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXTOG-001** MUST: Dynamic social preview and Open Graph card endpoints MUST use the next/og module to generate image responses directly from component-based layouts.

### Verify

```bash
# Discover the workspace test execution script in the project manifest and run endpoint integration tests targeting image generation routes.
# Identify the workspace type-checking and linting configuration from the build setup and run validation across image endpoint modules.
```

**Accept when:**
- All dynamic image route endpoints resolve next/og imports and compile without type or layout errors.
- Automated test suites verify image endpoints return appropriate content-type headers and handle missing query parameters gracefully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>