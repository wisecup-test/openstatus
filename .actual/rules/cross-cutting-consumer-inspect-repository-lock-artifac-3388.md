---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-3388
description: "Zod Schema Validation for Notification Configurations: Consumer Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 3388203a-f1c5-4d2c-82a9-29471932aad7
    status: accepted
    candidate_group_id: 53081f80-63f1-4a28-a21c-a5dc14dbb968
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Schema Validation for Notification Configurations: Consumer Inspect Repository Lock Artifact Determine

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: The consumer MUST inspect the repository lock artifact to determine the exact resolved validation library version before implementation and verify schema builder APIs against that version documentation.

### Verify

```bash
# Discover and execute the repository type-checking script across integration modules to verify schema typings
# Discover and run the project test suite covering notification configuration parsing and validation
```

**Accept when:**
- All notification configuration schemas pass static type verification and schema validation tests without errors.
- Integration configuration parsing succeeds with default endpoint fallbacks when optional credentials are omitted.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>