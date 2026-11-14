---
version: 2
name: cross-cutting-outbound-requests-twilio-messages-authen-e850
description: "Twilio Messages API Direct Integration: Outbound Requests Twilio Messages Authenticate Basic"
metadata:
  actual:
    adr_id: e8500cee-04b5-41fa-b8df-b34ff74c2f72
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: Outbound Requests Twilio Messages Authenticate Basic

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for outbound SMS notification dispatch modules interacting with external telephony providers.

### Rules

- **R-TWILIO-001** MUST: Outbound requests to the Twilio Messages API MUST authenticate using Basic authorization headers derived from external account credentials.

### Verify

```bash
# Discover and run the project static analysis and test validation suites from the repository root
# Discover and execute contract tests verifying outbound HTTP request serialization against external provider specifications
```

**Accept when:**
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>