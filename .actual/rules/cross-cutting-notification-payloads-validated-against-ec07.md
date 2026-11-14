---
version: 2
name: cross-cutting-notification-payloads-validated-against-ec07
description: "Twilio Messages API Direct Integration: Notification Payloads Validated Against Designated Schema"
metadata:
  actual:
    adr_id: ec0798fc-357a-41d4-aa20-433d1edf700e
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: Notification Payloads Validated Against Designated Schema

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for outbound SMS notification dispatch modules interacting with external telephony providers.

### Rules

- **R-SMS-001** MUST: Notification payloads MUST be validated against the designated schema before constructing outbound external API requests.

### Verify

```bash
# Discover and run project static analysis, test validation suites, and contract tests verifying outbound HTTP request serialization from the repository root.
```

**Accept when:**
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>