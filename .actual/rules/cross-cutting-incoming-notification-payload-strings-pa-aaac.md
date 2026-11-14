---
version: 2
name: cross-cutting-incoming-notification-payload-strings-pa-aaac
description: "Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Incoming Notification Payload Strings Parsed Validated"
metadata:
  actual:
    adr_id: aaacf77d-dc23-4ac8-b70d-a2dc2a1ce80d
    status: accepted
    candidate_group_id: 9bd500d7-caf0-4fe0-ae1e-44d97d6b6b6c
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Dispatch Handlers: Incoming Notification Payload Strings Parsed Validated

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ON-001** MUST: Incoming notification payload strings MUST be parsed and validated using googleChatDataSchema.parse prior to initiating external client dispatch operations.

### Verify

\`\`\`bash
# Discover and run the project's static analysis, type-checking, and unit/integration tests from the build configuration
npm test -- --run
\`\`\`

**Accept when:**
- Incoming notification payloads pass schema validation before triggering network calls.
- Outbound HTTP POST requests successfully serialize JSON content to target webhook endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations will cause pull request rejection during review.
</enforcement>