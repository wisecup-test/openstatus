---
version: 2
name: cross-cutting-internal-domain-module-expose-pure-b11e
description: "Internal Domain Module Adoption: Internal Domain Module Expose Pure Helper"
metadata:
  actual:
    adr_id: b11efd82-1bd1-42af-8d56-26638937dd3f
    status: accepted
    candidate_group_id: 5fa57055-2509-4adb-8d7d-503b417b3140
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/**
---

# Internal Domain Module Adoption: Internal Domain Module Expose Pure Helper

Scope: applies to `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all application subsystems requiring domain entity operations, custom domain resolution, or authentication domain verification.

### Rules

- **R-IDM-001** MAY: The internal domain module MAY expose pure helper functions and typed domain abstractions to assist consuming transport modules without leaking storage or provider-specific details.

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