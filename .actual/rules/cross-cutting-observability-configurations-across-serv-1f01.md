---
version: 2
name: cross-cutting-observability-configurations-across-serv-1f01
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Observability Configurations Across Server Edge Runtimes"
metadata:
  actual:
    adr_id: 1f01e800-1909-4b06-983d-5fa37870247c
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

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Observability Configurations Across Server Edge Runtimes

Scope: applies to `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Observability configurations across server and edge runtimes MUST apply error filtering criteria derived exclusively from @openstatus/api/src/trpc-errors.

### Verify

```bash
# Discover workspace dependency validation script and execute it to confirm all error module imports resolve to the shared package
# Discover and execute project static analysis and type verification directives to validate error handling conformity
```

**Accept when:**
- Static analysis confirms all error references in route handlers and observability configurations resolve to @openstatus/api/src/trpc-errors.
- No local error classification classes or duplicate error mapping functions exist within runtime configuration boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>