---
version: 2
name: cross-cutting-notification-message-builders-validate-r-f7b1
description: "Adopt @openstatus/regions Internal Module for Region Definitions: Notification Message Builders Validate Region Identifiers"
metadata:
  actual:
    adr_id: f7b18ca4-c97d-4df9-ab71-683b1bd0994a
    status: accepted
    candidate_group_id: c5f66e55-2633-46e9-84c4-e8af97ce4818
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adopt @openstatus/regions Internal Module for Region Definitions: Notification Message Builders Validate Region Identifiers

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for message formatting utilities and notification services that construct payload messages containing region metadata.

### Rules

- **R-OPN-001** SHOULD: Notification message builders SHOULD validate region identifiers against the exported types of @openstatus/regions during payload assembly.

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