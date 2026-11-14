---
version: 2
name: cross-cutting-consumers-discover-ecosystem-lock-artifa-f79f
description: "Adopt @openstatus/regions Internal Module for Region Definitions: Consumers Discover Ecosystem Lock Artifact Verify"
metadata:
  actual:
    adr_id: f79f282d-6a1d-440f-a4d2-dbd0b83a8e3b
    status: accepted
    candidate_group_id: c5f66e55-2633-46e9-84c4-e8af97ce4818
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adopt @openstatus/regions Internal Module for Region Definitions: Consumers Discover Ecosystem Lock Artifact Verify

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for all message formatting utilities, notification services, and code handling regional infrastructure metadata.

### Rules

- **R-REG-001** MUST: Consumers MUST discover the ecosystem lock artifact and verify the exact resolved version and module contract for all shared workspace dependencies prior to implementation.
- **R-REG-002** MUST: Import region definitions directly from @openstatus/regions and compose them into notification messages alongside incident timestamps and descriptions.
- **R-REG-003** MUST: Verify that all region lookups adhere to the exported interfaces of the shared module before dispatching formatted messages.

### Verify

```bash
test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured type checking script"
test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured test script"
```

**Accept when:**
- The discovered project verification suite completes with zero exit code across all notification modules.
- All references to region identifiers and metadata resolve strictly to @openstatus/regions without local duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, type verification, and peer code review enforce that no hardcoded regional strings are introduced in message formatting logic.
</enforcement>