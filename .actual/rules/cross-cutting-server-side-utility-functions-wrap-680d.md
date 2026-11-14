---
version: 2
name: cross-cutting-server-side-utility-functions-wrap-680d
description: "Adoption of next/headers for Server-Side Request Context Management: Server Side Utility Functions Wrap Next"
metadata:
  actual:
    adr_id: 680d085a-0173-4253-a91f-58eaf0eec5a8
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

# Adoption of next/headers for Server-Side Request Context Management: Server Side Utility Functions Wrap Next

Scope: applies to `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for server-side utility modules accessing incoming HTTP request headers or cookies, and server-side API client callers propagating request context to internal services.

### Rules

- **R-HEADER-001** MAY: Server-side utility functions MAY wrap next/headers accessors with fallback logic to return default values when invoked outside an active request context.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify next/headers imports are restricted to server-side modules.
npm run lint
# Discover and run the project test suite to confirm that server request context utilities correctly retrieve and sanitize headers and cookies under mock request environments.
npm test
```

**Accept when:**
- Static analysis confirms all imports of next/headers reside within designated server execution boundaries with zero client-side imports.
- Automated test executions verify that header forwarding routines strip hop-by-hop transport headers and cookie parsing validates input against schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures for prohibited client-side imports of next/headers block continuous integration builds.
</enforcement>