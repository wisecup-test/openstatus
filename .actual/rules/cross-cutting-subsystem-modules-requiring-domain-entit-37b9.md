---
version: 2
name: cross-cutting-subsystem-modules-requiring-domain-entit-37b9
description: "Internal Domain Module Adoption: Subsystem Modules Requiring Domain Entity Resolution"
metadata:
  actual:
    adr_id: 37b94d82-9928-4f2d-837b-22eaac292fdf
    status: accepted
    candidate_group_id: 5fa57055-2509-4adb-8d7d-503b417b3140
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/**
---

# Internal Domain Module Adoption: Subsystem Modules Requiring Domain Entity Resolution

Scope: applies to `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for application subsystems requiring domain entity operations, custom domain resolution, or authentication domain verification.

### Rules

- **R-DOM-001** MUST: Subsystem modules requiring domain entity resolution, authentication verification, or custom routing rewrites MUST consume business logic exclusively through the internal domain module.

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