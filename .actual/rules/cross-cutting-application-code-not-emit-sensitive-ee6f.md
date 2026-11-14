---
version: 2
name: cross-cutting-application-code-not-emit-sensitive-ee6f
description: "Console API Error and Warning Logging: Application Code Not Emit Sensitive Credentials"
metadata:
  actual:
    adr_id: ee6f1ed8-ddef-4143-a921-a76bbe5a8462
    status: accepted
    candidate_group_id: 91834650-166c-4203-af56-53c9938d4f03
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Console API Error and Warning Logging: Application Code Not Emit Sensitive Credentials

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONS-001** MUST_NOT: Application code MUST NOT emit sensitive credentials, session tokens, or unredacted personal identifiers into diagnostic calls through console.error or console.warn.

### Verify

```bash
# Discover and run the project static analysis and linting scripts from repository configuration
# Discover and execute automated test suites defined in the repository manifest
```

**Accept when:**
- Static analysis checks pass with zero unapproved console statements emitting sensitive tokens.
- Automated test suites pass, verifying that error branches execute expected diagnostic reporting paths.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>