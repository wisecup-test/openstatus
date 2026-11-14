---
version: 2
name: cross-cutting-domain-logic-business-rules-entity-7ac0
description: "Internal Domain Module Adoption: Domain Logic Business Rules Entity Workflows"
metadata:
  actual:
    adr_id: 7ac01174-00e5-4d1c-bbeb-83f6a06cd685
    status: accepted
    candidate_group_id: 5fa57055-2509-4adb-8d7d-503b417b3140
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/**
---

# Internal Domain Module Adoption: Domain Logic Business Rules Entity Workflows

Scope: applies to `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all application subsystems requiring domain entity operations, custom domain resolution, or authentication domain verification.

### Rules

- **R-DOM-001** MUST_NOT: Domain logic, business rules, and entity workflows MUST NOT be duplicated across transport adapters, proxy rewrites, or authentication providers.

### Verify

```bash
echo "Discover and execute the repository static analysis suite to verify internal domain module import boundaries"
echo "Discover and execute test runner suites validating proxy routing and authentication domain contracts"
```

**Accept when:**
- All proxy routing and authentication modules resolve domain logic exclusively through the internal domain module without rule duplication.
- Discovered static analysis and test runner suites execute with zero errors across all domain consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and code reviews during pull request verification enforce these rules, and violations will block continuous integration pipelines.
</enforcement>