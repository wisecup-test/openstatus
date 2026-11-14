---
version: 2
name: cross-cutting-before-implementing-modifying-domain-int-6d04
description: "Internal Domain Module Adoption: Before Implementing Modifying Domain Interactions Relying"
metadata:
  actual:
    adr_id: 6d043bbd-0676-4be6-b8b6-f774316b8cb1
    status: accepted
    candidate_group_id: 5fa57055-2509-4adb-8d7d-503b417b3140
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/**
---

# Internal Domain Module Adoption: Before Implementing Modifying Domain Interactions Relying

Scope: applies to `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DOM-001** MUST: Before implementing or modifying domain interactions relying on external runtime dependencies, the consumer MUST inspect repository lock artifacts to verify and bind to exact resolved dependency versions.

### Verify

```bash
echo "Discover and execute the repository static analysis suite to verify internal domain module import boundaries"
echo "Discover and execute test runner suites validating proxy routing and authentication domain contracts"
```

**Accept when:**
- All proxy routing and authentication modules resolve domain logic exclusively through the internal domain module without rule duplication.
- Discovered static analysis and test runner suites execute with zero errors across all domain consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>