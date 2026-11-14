---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-a004
description: "Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Engineers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: a004dee1-336f-4216-9269-27112fa5ab87
    status: accepted
    candidate_group_id: 3672a080-6f79-4231-a813-e4a5d7bbedcf
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/**
---

# Zod Schema Definition for Runtime Boundary Validation and Data Contract Enforcement: Engineers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all incoming data payloads, external API responses, environment variables, and system boundary configurations.

### Rules

- **R-ZOD-001** MUST: Engineers MUST inspect the repository lock artifact to determine the exact resolved dependency version of the validation library before implementing or upgrading schema definitions.

### Verify

```bash
# Execute repository type-checking pipeline
npx tsc --noEmit

# Execute test runner for validation verification
npm test

# Execute linting and static analysis suite
npm run lint
```

**Accept when:**
- All boundary validation schemas successfully compile without type mismatches between inferred schemas and consuming domain functions.
- Test suites verify that malformed payloads are rejected with structured diagnostics and valid payloads are accepted.
- No unvalidated external inputs or unchecked type assertions exist at application entry boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>