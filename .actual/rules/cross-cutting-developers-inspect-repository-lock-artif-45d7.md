---
version: 2
name: cross-cutting-developers-inspect-repository-lock-artif-45d7
description: "Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Developers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 45d7a064-89c9-4809-8eb5-eb72abb3978e
    status: accepted
    candidate_group_id: 46c9d68e-5833-432a-8164-b85f8973d04a
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Developers Inspect Repository Lock Artifact Verify

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for all incoming notification payload handling routines and deserialization of external or persisted notification data prior to outbound integration requests.

### Rules

- **R-VAL-001** MUST: Developers MUST inspect the repository lock artifact to verify the exact resolved version of internal and external validation dependencies before implementing or modifying schema validation routines.

### Verify

```bash
# Discover the test runner from the project manifest and run automated tests targeting notification payload validation suites.
# Discover the static analysis and type verification script from the repository configuration and execute project verification.
```

**Accept when:**
- All schema validation test cases covering valid and malformed payload inputs pass without regression.
- Static type verification and lint analysis succeed across all notification packages without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests lacking runtime schema validation on deserialized integration payloads will be blocked during review.
</enforcement>