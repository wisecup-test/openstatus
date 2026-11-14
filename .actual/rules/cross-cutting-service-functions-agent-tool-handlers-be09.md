---
version: 2
name: cross-cutting-service-functions-agent-tool-handlers-be09
description: "Zod Schema-Based Input Validation at Service Boundaries: Service Functions Agent Tool Handlers Validate"
metadata:
  actual:
    adr_id: be0986c0-57c2-47f5-aaa5-597200e3b5b9
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Service Functions Agent Tool Handlers Validate

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Service functions and agent tool handlers MUST validate all incoming caller arguments against a defined Zod schema using parse or safeParse before executing business logic or persistence transactions.

### Verify

```bash
# Discover the repository build, type-checking, test, and lint scripts from the package manifest and execute them:
# Example (run via discovered project commands):
# npm run build
# npm test
# npm run lint
```

**Accept when:**
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new or modified service entry points must parse inputs against defined schemas, and pull requests bypassing validation are blocked from merging.
</enforcement>