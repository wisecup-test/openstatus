---
version: 2
name: cross-cutting-engineers-consult-repository-lock-artifa-2054
description: "Adoption of next/server for Server Route and Request Handling: Engineers Consult Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 20549bd4-542b-48c1-a023-d7ee8a145abf
    status: accepted
    candidate_group_id: 2dd81b33-c7f2-44a9-87ad-46fa4c700394
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of next/server for Server Route and Request Handling: Engineers Consult Repository Lock Artifact Verify

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Engineers MUST consult the repository lock artifact to verify the exact resolved version of the server module dependency before introducing or modifying server endpoint logic.

### Verify

```bash
# Discover the project test runner from repository configuration and execute the server test suite.
# Discover the project type checker from repository configuration and verify type compliance across server route handlers.
# Discover the project linter from repository configuration and execute static analysis across server modules.
```

**Accept when:**
- All server route handlers and proxy modules compile without type errors using the server module interfaces.
- HTTP endpoint test suites pass successfully, verifying header extraction and cookie lifecycle management.
- Static analysis confirms no unauthorized direct transport mutations or unsupported runtime socket access in server route handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>