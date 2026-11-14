---
version: 2
name: cross-cutting-components-classifying-handling-reportin-379b
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Components Classifying Handling Reporting Errors Import"
metadata:
  actual:
    adr_id: 379b4e94-7d10-4264-98f7-0d341d717f7f
    status: accepted
    candidate_group_id: 78a52675-2a86-4000-bdbd-889f7e086ee9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/app/api/trpc/lambda/**
---

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Components Classifying Handling Reporting Errors Import

Scope: applies to `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: Components classifying, handling, or reporting API errors MUST import error definitions and categorization logic directly from @openstatus/api/src/trpc-errors.

### Verify

```bash
# Discover and execute workspace dependency validation script to confirm all error module imports resolve to the shared package
# Discover and execute project static analysis and type verification directives
```

**Accept when:**
- Static analysis confirms all error references in route handlers and observability configurations resolve to @openstatus/api/src/trpc-errors.
- No local error classification classes or duplicate error mapping functions exist within runtime configuration boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>