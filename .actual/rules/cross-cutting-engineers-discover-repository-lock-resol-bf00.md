---
version: 2
name: cross-cutting-engineers-discover-repository-lock-resol-bf00
description: "Adoption of next/og for Dynamic Open Graph Image Generation: Engineers Discover Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: bf00cebe-85d2-434f-9fc2-3495d372c66e
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

# Adoption of next/og for Dynamic Open Graph Image Generation: Engineers Discover Repository Lock Resolution Artifact

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOG-001** MUST: Engineers MUST discover the repository lock or resolution artifact and verify the exact resolved version of the image generation library before implementation.

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