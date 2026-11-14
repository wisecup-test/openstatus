---
version: 2
name: cross-cutting-modules-generating-notification-messages-5a8e
description: "Adopt @openstatus/regions Internal Module for Region Definitions: Modules Generating Notification Messages Import Region"
metadata:
  actual:
    adr_id: 5a8e6fc8-69e6-4bc9-95c0-2655174af2b8
    status: accepted
    candidate_group_id: c5f66e55-2633-46e9-84c4-e8af97ce4818
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adopt @openstatus/regions Internal Module for Region Definitions: Modules Generating Notification Messages Import Region

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** MUST: Modules generating notification messages MUST import region identifiers and metadata directly from @openstatus/regions.

### Verify

```bash
test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured type checking script"
test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured test script"
```

**Accept when:**
- The discovered project verification suite completes with zero exit code across all notification modules.
- All references to region identifiers and metadata resolve strictly to @openstatus/regions without local duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>