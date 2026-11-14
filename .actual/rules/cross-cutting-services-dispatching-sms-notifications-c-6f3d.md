---
version: 2
name: cross-cutting-services-dispatching-sms-notifications-c-6f3d
description: "Twilio Messages API Direct Integration: Services Dispatching Sms Notifications Communicate Twilio"
metadata:
  actual:
    adr_id: 6f3db24d-5841-4f2d-8aa2-3243dca3f9d7
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: Services Dispatching Sms Notifications Communicate Twilio

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for outbound SMS notification dispatch modules interacting with external telephony providers.

### Rules

- **R-TMA-001** MUST: Services dispatching SMS notifications MUST communicate with the Twilio Messages API via authenticated HTTP requests transmitting URL-encoded message payloads.

### Verify

```bash
# Discover and run the project static analysis and test validation suites from the repository root
# Discover and execute contract tests verifying outbound HTTP request serialization against external provider specifications
```

**Accept when:**
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced by automated continuous integration pipeline checks and peer code review.
</enforcement>