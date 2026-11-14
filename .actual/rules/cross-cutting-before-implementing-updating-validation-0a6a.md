---
version: 2
name: cross-cutting-before-implementing-updating-validation-0a6a
description: "Zod Schema-Based Input Validation at Service Boundaries: Before Implementing Updating Validation Schemas Zod"
metadata:
  actual:
    adr_id: 0a6a51e0-b242-4d65-947c-f4f287d0ff6f
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Before Implementing Updating Validation Schemas Zod

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Before implementing or updating validation schemas with Zod, consumers MUST locate the repository dependency manifest and inspect the repository lock artifact to determine the exact locked version of Zod, verifying all schema methods against official documentation for that version.

### Verify

```bash
# Discover and execute the repository build, type-checking, test, and static analysis scripts from the package manifest
# Example typical commands (derived from project files):
npm run build || pnpm build || yarn build
npm run test || pnpm test || yarn test
npm run lint || pnpm lint || yarn lint
```

**Accept when:**
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new or modified service entry points must parse inputs against defined schemas, and all verification scripts must pass without discrepancies.
</enforcement>