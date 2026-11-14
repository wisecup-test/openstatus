---
version: 2
name: cross-cutting-failures-encountered-during-analytics-ev-b6f4
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Failures Encountered During Analytics Event Emission"
metadata:
  actual:
    adr_id: b6f4cba3-1895-4b0b-ae19-6a3e16f92e17
    status: accepted
    candidate_group_id: cf93de28-0c1e-48d4-8854-47c3358b8b7e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
      - src/routes/**
---

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Failures Encountered During Analytics Event Emission

Scope: applies to `apps/server/src/libs/middlewares/**`, `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all server-side REST route handlers, RPC interceptors, and HTTP middlewares that process incoming requests or perform domain mutations.

### Rules

- **R-ANALYTICS-001** SHOULD: Failures encountered during analytics event emission SHOULD be caught and logged as non-fatal warnings to prevent telemetry disruptions from failing primary request transactions.
- **R-ANALYTICS-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ANALYTICS-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that exact version's documentation, and re-run per dependency at point of use for version-sensitive behavior.
- **R-ANALYTICS-004** MANDATORY: Extract the active workspace identifier from the request context before invoking analytics dispatch routines.
- **R-ANALYTICS-005** MANDATORY: Catch analytics delivery errors and log them through the structured logging facility at warning level with event and workspace context attributes.

### Verify

```bash
# Discover the workspace static analysis and type check scripts from the repository manifests and execute them across the server application.
# Discover and run the test suite scripts for server routes and interceptors to verify analytics event dispatch handling and error isolation.
```

**Accept when:**
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code reviews.
</enforcement>