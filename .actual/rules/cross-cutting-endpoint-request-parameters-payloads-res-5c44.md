---
version: 2
name: cross-cutting-endpoint-request-parameters-payloads-res-5c44
description: "Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Endpoint Request Parameters Payloads Response Bodies"
metadata:
  actual:
    adr_id: 5c446065-862f-4596-b63f-deb9e50206c9
    status: accepted
    candidate_group_id: ab635fa0-3163-43ac-b634-a36e9733e3a2
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/**
      - src/routes/**
---

# Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Endpoint Request Parameters Payloads Response Bodies

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all HTTP route definitions, API endpoints, webhook receivers, and request-scoped middleware within the server application.

### Rules

- **R-HONO-001** SHOULD: Endpoint request parameters, request payloads, and response bodies SHOULD be validated using schema adapters integrated with the framework routing layer.

### Verify

```bash
# Discover and execute the repository dependency audit script to verify all framework dependencies resolve to approved lock entries.
# Discover and execute the repository static analysis and type checking scripts to verify framework route and context typing compliance.
# Discover and execute the automated test runner to ensure all HTTP endpoint handlers and middleware pipelines pass validation.
```

**Accept when:**
- All server route handlers and middleware compile without type errors against the resolved framework context type contracts.
- Automated integration and unit test suites pass, verifying route dispatching and middleware chain execution.
- Static analysis confirms absence of unmanaged global request state in favor of framework context storage.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated type checking, linting rules, and peer architecture reviews are enforced during continuous integration pipelines.
</enforcement>