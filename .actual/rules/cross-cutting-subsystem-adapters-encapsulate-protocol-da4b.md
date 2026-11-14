---
version: 2
name: cross-cutting-subsystem-adapters-encapsulate-protocol-da4b
description: "Internal Domain Module Adoption: Subsystem Adapters Encapsulate Protocol Specific Headers"
metadata:
  actual:
    adr_id: da4be14f-5cc4-46e9-b2c7-310ccdcc3412
    status: accepted
    candidate_group_id: 5fa57055-2509-4adb-8d7d-503b417b3140
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/**
---

# Internal Domain Module Adoption: Subsystem Adapters Encapsulate Protocol Specific Headers

Scope: applies to `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all application subsystems requiring domain entity operations, custom domain resolution, or authentication domain verification.

### Rules

- **R-DOM-001** SHOULD: Subsystem adapters SHOULD encapsulate protocol-specific headers, rewrites, and session states, delegating all domain state evaluations to the internal domain module.

### Verify

```bash
echo "Discover and execute the repository static analysis suite to verify internal domain module import boundaries"
echo "Discover and execute test runner suites validating proxy routing and authentication domain contracts"
```

**Accept when:**
- All proxy routing and authentication modules resolve domain logic exclusively through the internal domain module without rule duplication.
- Discovered static analysis and test runner suites execute with zero errors across all domain consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, code reviews, and test suites verify compliance. Violations will block continuous integration pipelines.
</enforcement>