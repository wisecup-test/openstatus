---
version: 2
name: cross-cutting-consumers-discover-repository-dependency-4a3c
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Consumers Discover Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 4a3cd5c4-889e-4296-a08b-a463e670e9f8
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

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Consumers Discover Repository Dependency Resolution Artifact

Scope: applies to `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/app/api/trpc/lambda/**`.

These rules are ALWAYS ACTIVE for API route handlers, endpoint transport adapters, and server and edge runtime observability and monitoring configuration modules.

### Rules

- **R-ERR-001** MUST: Consumers MUST discover the repository dependency resolution artifact and verify the exact locked version of internal and external dependencies before modifying integration code.
- **R-ERR-002** MUST: Exported error structures MUST remain isomorphic and avoid importing runtime-specific dependencies incompatible with edge environments.
- **R-ERR-003** MUST: Error filtering configurations in monitoring layers MUST coordinate with the status classifications exported by the shared module.

### Verify

```bash
# Discover and execute the workspace dependency validation script
# Discover and execute the project static analysis and type verification directives
```

**Accept when:**
- Static analysis confirms all error references in route handlers and observability configurations resolve to @openstatus/api/src/trpc-errors.
- No local error classification classes or duplicate error mapping functions exist within runtime configuration boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration type checking, static analysis suites, and peer code review.
</enforcement>