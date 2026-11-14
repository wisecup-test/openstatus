---
version: 2
name: cross-cutting-search-parameter-parser-schemas-centrali-ce25
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Search Parameter Parser Schemas Centralized Dedicated"
metadata:
  actual:
    adr_id: ce258eae-e2bb-48be-a7e2-c31de7f14e14
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Search Parameter Parser Schemas Centralized Dedicated

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all server-rendered route page components that receive and process URL search parameters and shared search parameter definitions consumed by both server page handlers and client components.

### Rules

- **R-NQS-001** SHOULD: Search parameter parser schemas SHOULD be centralized in dedicated parameter modules to ensure schema parity between server-side validation and client-side URL state management.
- **R-NQS-002** MANDATORY: The consumer MUST derive build tools, file names, commands, package managers, and versions from the project repository.
- **R-NQS-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that exact version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-NQS-004** MANDATORY: Colocate search parameter schema parser cache definitions alongside route handlers or in dedicated parameter modules to facilitate reuse between server and client boundaries.
- **R-NQS-005** MANDATORY: Pass parsed parameter cache instances to client components rather than forwarding unvalidated raw query parameter objects.

### Verify

```bash
# Discover and execute the repository static type checker to confirm search parameter parsing calls adhere to defined schema types
# Discover and run the project test suite to verify route parameter parsing and validation error handling
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, linting rules, and peer code reviews verify adherence, leading to build failure or PR rejection upon unparsed query parameter consumption.
</enforcement>