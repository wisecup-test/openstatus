---
version: 2
name: cross-cutting-modules-consuming-sensitive-third-party-4911
description: "Next.js Process Environment Secrets and Credentials Handling: Modules Consuming Sensitive Third Party Service"
metadata:
  actual:
    adr_id: 491187d3-e705-4be0-9f91-623d8cfca469
    status: accepted
    candidate_group_id: 7c7309e0-f7a5-478c-9be6-6a5a8311816d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Next.js Process Environment Secrets and Credentials Handling: Modules Consuming Sensitive Third Party Service

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXT-001** SHOULD: Modules consuming sensitive third-party service credentials SHOULD isolate environment lookups within dedicated server-side adapter or provider modules rather than referencing process environment properties in generic utilities.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts
# Example: npm run lint

# Discover and execute the project's build and compilation scripts
# Example: npm run build
```

**Accept when:**
- Static analysis checks report zero unauthorized process environment accesses in client compilation units.
- Application build and type verification succeed with clear separation between server and client configuration bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>