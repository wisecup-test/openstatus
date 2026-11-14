---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-8a7d
description: "Adoption of @openstatus/notification-base for Notification Providers: Engineers Discover Repository Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: 8a7d76b7-5d89-450e-b84c-9e608e8f9d4c
    status: accepted
    candidate_group_id: 669b5cd4-f03b-4a56-83c2-fc08a7b061d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Providers: Engineers Discover Repository Dependency Manifest Inspect

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for packages and modules implementing notification delivery channels, alert dispatch, and code responsible for validating and transforming incoming notification event payloads.

### Rules

- **R-NOT-001** MUST: Engineers MUST discover the repository dependency manifest and inspect the repository lock artifact to verify the exact resolved version of @openstatus/notification-base prior to implementation.

### Verify

```bash
# Discover and run the repository typecheck script to verify type compatibility with the base notification module across packages.
# Discover and run the workspace test runner to validate notification payload handling and dispatch flows.
# Discover and execute the repository linter script to ensure compliance with module import and boundary rules.
```

**Accept when:**
- All notification provider implementations compile cleanly against the shared base module interfaces and pass static type verification.
- Automated unit and integration test suites validating notification payload parsing and dispatch execute with zero failures.
- Payload validation schemas successfully parse valid notification inputs and reject malformed structures prior to dispatch.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification provider implementations must adhere to the base module contracts and separation of dispatch boundaries, verified via automated CI checks and peer code review.
</enforcement>