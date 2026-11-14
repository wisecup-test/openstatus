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

These rules are ALWAYS ACTIVE for all incoming HTTP server route handlers, query parameter parsers, request payload consumers, external client responses received from remote service integrations and third-party APIs, application environment variable definitions and configuration loaders, content parsing, search indexing, and domain-level structured schemas.

### Rules

- **R-ZOD-001** SHOULD: Engineers derive static domain types directly from runtime Zod schemas using schema inference rather than maintaining standalone manual type declarations.

### Verify

```bash
# Execute repository type-checking pipeline
# (Command to be discovered from workspace configuration)

# Execute test runner
# (Command to be discovered from project automation scripts)

# Execute linting and static analysis suite
# (Command to be discovered from repository configuration)
```

**Accept when:**
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and new code must be verified using the repository's discovered type-checking, testing, and linting suites.
</enforcement>