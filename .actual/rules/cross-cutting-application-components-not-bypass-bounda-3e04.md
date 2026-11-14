---
version: 2
name: cross-cutting-application-components-not-bypass-bounda-3e04
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Application Components Not Bypass Boundary Schema"
metadata:
  actual:
    adr_id: 3e04420f-2dcb-43e9-a101-d61789cd2b36
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Application Components Not Bypass Boundary Schema

Scope: applies to `apps/web/src/**`.

These rules are ALWAYS ACTIVE for incoming HTTP server route handlers, query parameter parsers, request payload consumers, external client responses received from remote service integrations and third-party APIs, application environment variable definitions and configuration loaders, and content parsing, search indexing, and domain-level structured schemas.

### Rules

- **R-ZOD-001** MUST_NOT: Application components MUST NOT bypass boundary schema parsing or cast untrusted external payloads directly to internal domain types using unchecked type assertions.

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