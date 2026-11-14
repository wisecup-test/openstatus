---
version: 2
name: cross-cutting-service-modules-not-establish-isolated-62e1
description: "Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Service Modules Not Establish Isolated Direct"
metadata:
  actual:
    adr_id: 62e1e777-a070-4775-826d-56a0a0ccd7d5
    status: accepted
    candidate_group_id: ac7c8bf1-716e-4288-bcba-c365b3097035
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Service Modules Not Establish Isolated Direct

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all domain services, handlers, and business logic modules that perform reads, writes, updates, or transactional mutations on persistent relational data.

### Rules

- **R-DB-001** MUST_NOT: Service modules MUST NOT establish isolated direct database connections or declare duplicate schema table definitions outside the canonical module.

### Verify

```bash
# Inspect project configuration to discover the automated static analysis script, then run it
# Example discovery / execution pattern depending on project configuration:
# npm run lint / npx tsc --noEmit (or workspace-specific equivalents)
```

**Accept when:**
- Static analysis verifies zero direct database connections or duplicate table definitions exist outside the designated database module.
- All service-layer data mutations and queries execute successfully within automated test suites without schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>