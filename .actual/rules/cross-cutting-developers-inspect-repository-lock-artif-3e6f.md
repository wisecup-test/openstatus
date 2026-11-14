---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-3e6f
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Developers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 3e6f385a-5e07-4f3a-95f7-f99ba64e653d
    status: accepted
    candidate_group_id: 879cd289-00f5-4876-a548-936bf328492c
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Developers Inspect Repository Lock Artifact Determine

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for workspace packages and modules requiring runtime environment variable configuration.

### Rules

- **R-ENV-001** MUST: Developers MUST inspect the repository lock artifact to determine the exact resolved version of @t3-oss/env-core before authoring or modifying environment validation definitions.

### Verify

```bash
# Discover and run the project static type checker
# Discover and execute the package test suite
```

**Accept when:**
- Static type analysis passes with zero diagnostics on environment schema definitions.
- Automated test suites confirm that missing required environment variables cause immediate initialization failure with schema diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds will fail if environment variable schemas do not pass static type checking or test validation.
</enforcement>