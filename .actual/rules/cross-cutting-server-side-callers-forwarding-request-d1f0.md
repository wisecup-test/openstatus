---
version: 2
name: cross-cutting-server-side-callers-forwarding-request-d1f0
description: "Adoption of next/headers for Server-Side Request Context Management: Server Side Callers Forwarding Request Headers"
metadata:
  actual:
    adr_id: d1f0b361-7e8c-4ee5-81c4-0e3b7e2db163
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

# Adoption of next/headers for Server-Side Request Context Management: Server Side Callers Forwarding Request Headers

Scope: applies to `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** MUST: Server-side callers forwarding request headers to downstream services MUST sanitize the header collection by deleting hop-by-hop transport headers before dispatch.

### Verify

```bash
# Discover and execute repository static analysis and linting scripts to verify server-only import boundaries
# Discover and run the project test suite to verify header sanitization and request context utilities
```

**Accept when:**
- Static analysis confirms all imports of next/headers reside within designated server execution boundaries with zero client-side imports.
- Automated test executions verify that header forwarding routines strip hop-by-hop transport headers and cookie parsing validates input against schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>