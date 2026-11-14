---
version: 2
name: cross-cutting-developers-inspect-repository-lock-resol-6a4c
description: "@openstatus/notification-base Environment Secrets Handling: Developers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 6a4c41ba-1da8-4844-a088-ecb169f0372d
    status: accepted
    candidate_group_id: b721e395-9761-4cb0-a0a8-e052244b08ac
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# @openstatus/notification-base Environment Secrets Handling: Developers Inspect Repository Lock Resolution Artifact

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for notification adapters, dispatch modules integrating with external messaging services, and direct runtime credential resolution for outbound notification requests.

### Rules

- **R-NSB-001** MUST: Developers MUST inspect the repository lock or resolution artifact to confirm the exact resolved dependency versions before implementing or updating notification client integrations.

### Verify

```bash
# Discover and run the project's static analysis and test validation suites
# Execute repository code scanner to ensure no hardcoded secrets or exposed tokens exist
```

**Accept when:**
- All notification test suites pass successfully with environment variables supplied via test harnesses.
- Static analysis reports zero hardcoded secrets or unvalidated payload accesses in notification adapters.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must pass automated static analysis, secret scanning, and test validation.
</enforcement>