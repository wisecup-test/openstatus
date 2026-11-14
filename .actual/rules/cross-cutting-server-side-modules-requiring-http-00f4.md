---
version: 2
name: cross-cutting-server-side-modules-requiring-http-00f4
description: "Adoption of next/headers for Server-Side Request Context Management: Server Side Modules Requiring Http Request"
metadata:
  actual:
    adr_id: 00f46233-bde2-48cc-894a-b6f9fe56ab14
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

# Adoption of next/headers for Server-Side Request Context Management: Server Side Modules Requiring Http Request

Scope: applies to `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HTTP-001** MUST: Server-side modules requiring HTTP request metadata or client cookie values MUST retrieve request context through next/headers APIs rather than accepting manual request object parameters.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify that next/headers imports are restricted to server-side modules.
npm run lint

# Discover and run the project test suite to confirm that server request context utilities correctly retrieve and sanitize headers and cookies under mock request environments.
npm test
```

**Accept when:**
- Static analysis confirms all imports of next/headers reside within designated server execution boundaries with zero client-side imports.
- Automated test executions verify that header forwarding routines strip hop-by-hop transport headers and cookie parsing validates input against schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>