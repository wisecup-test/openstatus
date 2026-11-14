---
version: 2
name: cross-cutting-procedure-definitions-utilizing-trpc-ser-54c8
description: "Evaluation of after Callback within @trpc/server Middleware: Procedure Definitions Utilizing Trpc Server Middleware"
metadata:
  actual:
    adr_id: 54c84448-38f4-4893-993f-7dccb681eca6
    status: accepted
    candidate_group_id: 488a7432-7d16-4be8-9ce8-5827c5960bf3
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Evaluation of after Callback within @trpc/server Middleware: Procedure Definitions Utilizing Trpc Server Middleware

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for server-side procedure middleware pipelines defined with @trpc/server.

### Rules

- **R-TRPC-001** MUST_NOT: Procedure definitions utilizing @trpc/server middleware MUST NOT treat asynchronous post-execution after callbacks as unit test lifecycle constructs.

### Verify

```bash
# Discover and run the project test execution script from the repository manifest to verify procedure logic independently.
# Example discovery step using standard repository tooling:
npm test
```

**Accept when:**
- The project test verification scripts complete without relying on procedure middleware lifecycle hooks for test assertions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review and CI pipeline checks enforce clean separation of runtime middleware and test suites. Pull requests attempting to embed test lifecycle hooks into production procedure middleware are rejected.
</enforcement>