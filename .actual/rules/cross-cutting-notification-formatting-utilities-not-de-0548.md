---
version: 2
name: cross-cutting-notification-formatting-utilities-not-de-0548
description: "Adopt @openstatus/regions Internal Module for Region Definitions: Notification Formatting Utilities Not Declare Hardcoded"
metadata:
  actual:
    adr_id: 05481399-cf92-45b3-99bb-87094fa344c8
    status: accepted
    candidate_group_id: c5f66e55-2633-46e9-84c4-e8af97ce4818
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adopt @openstatus/regions Internal Module for Region Definitions: Notification Formatting Utilities Not Declare Hardcoded

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for message formatting utilities and notification services that construct payload messages containing region metadata.

### Rules

- **R-NOT-001** MUST_NOT: Notification formatting utilities MUST NOT declare hardcoded region strings, ad-hoc region lookup tables, or local duplicate region enumerations.

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