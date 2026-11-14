---
version: 2
name: cross-cutting-engineers-derive-static-domain-types-857d
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Engineers Derive Static Domain Types Directly"
metadata:
  actual:
    adr_id: 857d6ea9-6166-48ef-921b-15e0573f4fec
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Engineers Derive Static Domain Types Directly

Scope: applies to `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all incoming HTTP server route handlers, query parameter parsers, request payload consumers, external client responses, application environment variable definitions, and content parsing modules.

### Rules

- **R-ZOD-001** SHOULD: Engineers derive static domain types directly from runtime Zod schemas using schema inference rather than maintaining standalone manual type declarations.
- **R-ZOD-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use for version-sensitive behavior.
- **R-ZOD-004** SHOULD: Place shared data validation schemas within dedicated schema modules accessible by both server routes and client services to maximize schema reuse.
- **R-ZOD-005** SHOULD: Prefer safeParse over parse in request handling paths to generate structured diagnostic responses rather than throwing caught exceptions.
- **R-ZOD-006** SHOULD: Use schema inference utilities provided by the library to export static types alongside schema objects to maintain a single source of truth.

### Verify

```bash
# Execute the repository type-checking pipeline discovered from workspace configuration
# Execute the test runner discovered from project automation scripts
# Execute the linting and static analysis suite discovered from repository configuration
```

**Accept when:**
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>