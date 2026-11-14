---
version: 2
name: cross-cutting-route-handlers-webhook-endpoints-context-6d23
description: "Adoption of @openstatus/services Module for Centralized Domain Logic: Route Handlers Webhook Endpoints Context Middleware"
metadata:
  actual:
    adr_id: 6d233f00-d6e7-496e-95a7-e9262e384b15
    status: accepted
    candidate_group_id: d7fa39ec-eb90-4ffa-b65a-12044e6ce4ef
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/services Module for Centralized Domain Logic: Route Handlers Webhook Endpoints Context Middleware

Scope: applies to `apps/dashboard/src/app/api/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for all application route handlers, edge context resolvers, webhook consumers, and agent tooling consuming domain business capabilities, shared domain logic, external service integrations, and business workflow orchestration.

### Rules

- **R-SRV-001** MUST_NOT: Route handlers, webhook endpoints, and context middleware MUST NOT directly execute unencapsulated business logic or bypass domain boundaries defined by @openstatus/services.

### Verify

```bash
# Discover the workspace dependency manifest and run repository type verification
# Discover and run the project test execution script targeting domain service modules and consuming endpoints
# Discover and execute the lint and module boundary validation suite
```

**Accept when:**
- Static type verification succeeds across all consumers of @openstatus/services without interface mismatches.
- All unit and integration test suites testing domain services and consumer endpoints pass with zero failures.
- Architecture boundary checks verify that domain logic in route handlers and agent tools is invoked exclusively through @openstatus/services.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, architecture linters, and peer code reviews enforce static types, boundary rules, and service tests.
</enforcement>