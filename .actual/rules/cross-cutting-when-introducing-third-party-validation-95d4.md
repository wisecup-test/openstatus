---
version: 2
name: cross-cutting-when-introducing-third-party-validation-95d4
description: "process.env Environment Variable Access: When Introducing Third Party Validation Configuration"
metadata:
  actual:
    adr_id: 95d44c7a-3ebf-4e92-8001-7e1137239dcd
    status: accepted
    candidate_group_id: 6362eb6e-0e78-4f8b-a64d-0762dee636ef
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# process.env Environment Variable Access: When Introducing Third Party Validation Configuration

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build and transformation scripts executing in the workspace that access runtime environment variables.

### Rules

- **R-ENV-001** MUST: When introducing third-party validation or configuration management libraries, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version prior to use.

### Verify

```bash
# Discover the workspace script verification target in the package manifest and execute the designated test or lint suite.
# Inspect build script sources to verify that all process.env accesses include existence checks or fallback handling.
```

**Accept when:**
- All build and transform scripts execute successfully across simulated environments without throwing unhandled exceptions for missing environment keys.
- Static analysis checks pass without reporting unvalidated secrets exposure in transformation scripts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code review.
</enforcement>