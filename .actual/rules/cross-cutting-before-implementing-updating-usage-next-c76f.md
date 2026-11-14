---
version: 2
name: cross-cutting-before-implementing-updating-usage-next-c76f
description: "Adoption of next/headers for Server-Side Request Context Management: Before Implementing Updating Usage Next Headers"
metadata:
  actual:
    adr_id: c76f2fd1-89df-4cee-9bab-4cb7718332d2
    status: accepted
    candidate_group_id: 1c61ef2b-7759-42f4-82a3-4ce67ddac0d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/lib/**
      - src/trpc/**
---

# Adoption of next/headers for Server-Side Request Context Management: Before Implementing Updating Usage Next Headers

Scope: applies to `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NH-001** MUST: Before implementing or updating usage of next/headers, the consumer MUST inspect the repository dependency manifest and resolution lock artifact to establish the exact resolved library version and verify API compatibility against official version-specific documentation.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify that next/headers imports are restricted to server-side modules.
# Discover and run the project test suite to confirm that server request context utilities correctly retrieve and sanitize headers and cookies under mock request environments.
```

**Accept when:**
- Static analysis confirms all imports of next/headers reside within designated server execution boundaries with zero client-side imports.
- Automated test executions verify that header forwarding routines strip hop-by-hop transport headers and cookie parsing validates input against schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All code modifications governed by this policy must be validated against the specified static analysis and test suites.
</enforcement>