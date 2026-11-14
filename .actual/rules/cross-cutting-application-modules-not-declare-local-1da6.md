---
version: 2
name: cross-cutting-application-modules-not-declare-local-1da6
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Application Modules Not Declare Local Error"
metadata:
  actual:
    adr_id: 1da6ce56-6462-415c-9909-2deab263bb5e
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

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Application Modules Not Declare Local Error

Scope: applies to `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST_NOT: Application modules MUST NOT declare local error classifications or duplicate error categorization predicates defined in @openstatus/api/src/trpc-errors.

### Verify

```bash
# Discover and execute the workspace dependency validation script to confirm all error module imports resolve to the shared package.
# Discover and execute the project static analysis and type verification directives to validate error handling conformity.
```

**Accept when:**
- Static analysis confirms all error references in route handlers and observability configurations resolve to @openstatus/api/src/trpc-errors.
- No local error classification classes or duplicate error mapping functions exist within runtime configuration boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>