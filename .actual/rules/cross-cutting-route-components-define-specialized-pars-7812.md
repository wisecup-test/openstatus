---
version: 2
name: cross-cutting-route-components-define-specialized-pars-7812
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Components Define Specialized Parser Cache"
metadata:
  actual:
    adr_id: 78125f07-d76d-47d5-ba12-f3c6ce07d2b7
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Components Define Specialized Parser Cache

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NUQS-001** MAY: Route components MAY define specialized parser cache instances for embedded or sub-domain views where parameter requirements differ from primary public routes.
- **R-NUQS-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository (DISCOVERY POLICY).
- **R-NUQS-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding process in order: find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm every API exists, and re-run per dependency at point of use.
- **R-NUQS-004** MANDATORY: Colocate search parameter schema parser cache definitions alongside route handlers or in dedicated parameter modules to facilitate reuse between server and client boundaries.
- **R-NUQS-005** MANDATORY: Pass parsed parameter cache instances to client components rather than forwarding unvalidated raw query parameter objects.

### Verify

```bash
# Discover and execute the repository static type checker to confirm that all search parameter parsing calls adhere to defined schema types.
# Discover and run the project test suite to verify route parameter parsing and validation error handling.
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, linting rules prohibiting direct unparsed query parameter consumption, and peer code review verification against centralized search parameter schema definitions.
</enforcement>