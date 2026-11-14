---
version: 2
name: cross-cutting-subsystems-extend-openstatus-regions-thr-578d
description: "Adopt @openstatus/regions Internal Module for Region Definitions: Subsystems Extend Openstatus Regions Through Centralized"
metadata:
  actual:
    adr_id: 578d6d43-6d6d-48b0-94af-413e85121cb3
    status: accepted
    candidate_group_id: c5f66e55-2633-46e9-84c4-e8af97ce4818
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adopt @openstatus/regions Internal Module for Region Definitions: Subsystems Extend Openstatus Regions Through Centralized

Scope: applies to `packages/notifications/base/src/utils/**`.

These rules are ALWAYS ACTIVE for message formatting utilities, notification services, and subsystems constructing payload messages containing region metadata.

### Rules

- **R-OPN-001** MAY: Subsystems MAY extend `@openstatus/regions` through centralized workspace pull requests when new monitoring locations or regions are introduced.
- **R-OPN-002** MUST: Import region definitions directly from `@openstatus/regions` and compose them into notification messages alongside incident timestamps and descriptions.
- **R-OPN-003** MUST: Verify that all region lookups adhere to the exported interfaces of the shared module before dispatching formatted messages.

### Verify

```bash
test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured type checking script"
test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured test script"
```

**Accept when:**
- The discovered project verification suite completes with zero exit code across all notification modules.
- All references to region identifiers and metadata resolve strictly to `@openstatus/regions` without local duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>