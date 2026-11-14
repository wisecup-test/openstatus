---
version: 2
name: cross-cutting-before-introducing-modifying-code-that-9b3f
description: "Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Before Introducing Modifying Code That References"
metadata:
  actual:
    adr_id: 9b3f3995-7016-4bc4-8ebb-868148cbc469
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

# Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Before Introducing Modifying Code That References

Scope: applies to `apps/server/src/libs/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Before introducing or modifying code that references the web framework, developers MUST inspect the repository dependency resolution lock artifact to identify and align with the exact resolved framework version.

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
Claude Code MUST NOT skip or defer verification. Automated type checking, linting, and continuous integration pipelines enforce these rules.
</enforcement>