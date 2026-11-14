---
version: 2
name: cross-cutting-when-external-libraries-are-integrated-cf17
description: "Adoption of Internal Utilities Module ./utils for Shared Operations: When External Libraries Are Integrated Internal"
metadata:
  actual:
    adr_id: cf1792b6-51ff-43ce-8f18-a7ae39315d6f
    status: accepted
    candidate_group_id: 607853f0-c585-4550-bd89-7cf890750abe
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of Internal Utilities Module ./utils for Shared Operations: When External Libraries Are Integrated Internal

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: When external libraries are integrated into internal utility modules, the engineer MUST inspect the project lock artifact to verify the exact resolved version against the project manifest before referencing external functions.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute test/lint suites
```

**Accept when:**
- All unit tests covering internal utility functions and their consumer modules pass without failures.
- Static analysis checks verify that tracking domain modules import common date helpers from the internal utilities module rather than defining duplicate local functions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>