---
version: 2
name: cross-cutting-before-implementing-code-depending-versi-5bd2
description: "Adoption of @openstatus/notification-base for Subscription Notification Channels: Before Implementing Code Depending Versioned Modules"
metadata:
  actual:
    adr_id: 5bd20069-922d-4b7b-96d3-ee128bf2ad69
    status: accepted
    candidate_group_id: 6dcad4eb-e237-4aa1-a010-3e5579a2bf71
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @openstatus/notification-base for Subscription Notification Channels: Before Implementing Code Depending Versioned Modules

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all subscription notification channels and message dispatch modules within the system.

### Rules

- **R-SUBS-001** MUST: Before implementing code depending on versioned modules, developers MUST locate the project resolution artifact, inspect the authoritative resolved dependency version, and verify API compatibility against the exact version documentation.
- **R-SUBS-002** MUST: All subscription notification channels and message dispatch modules import core notification contracts from @openstatus/notification-base rather than defining independent domain schemas.

### Verify

```bash
grep -rn "@openstatus/notification-base" .
find . -type f -exec grep -l "@openstatus/notification-base" {} +
```

**Accept when:**
- All notification channel modules import core notification contracts from @openstatus/notification-base rather than defining independent domain schemas.
- Automated project verification and type checking scripts execute successfully with zero dependency resolution errors.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>