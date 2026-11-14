---
version: 2
name: cross-cutting-route-handlers-webhook-processing-functi-926c
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Route Handlers Webhook Processing Functions Map"
metadata:
  actual:
    adr_id: 926c9480-a780-4e90-9c06-a2c4c15baf8f
    status: accepted
    candidate_group_id: 6a007d6a-909a-425c-8c5a-15faed6eeac3
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Route Handlers Webhook Processing Functions Map

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all application endpoints, route handlers, webhook receivers, client components, server rendering components, metadata generators, and auxiliary HTTP handlers querying domain entities via internal API interfaces.

### Rules

- **R-OPENSTATUS-001** SHOULD: Route handlers and webhook processing functions map and normalize all internal service errors using the error handling exports provided by the internal API package.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify that all procedure calls match module contracts.
# Discover and run the workspace test suite to ensure server route handlers and client callers integrate without contract violations.
npm run check || pnpm check || yarn check
npm test || pnpm test || yarn test
```

**Accept when:**
- All workspace type checks pass with zero signature or contract mismatch errors across all consuming modules.
- All route integration tests pass successfully without runtime errors during procedure invocation or error serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute workspace type verification and contract test suites, and architectural pull request reviews enforce module boundary compliance.
</enforcement>