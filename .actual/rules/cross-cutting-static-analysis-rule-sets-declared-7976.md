---
version: 2
name: cross-cutting-static-analysis-rule-sets-declared-7976
description: "Oxlint Static Code Analysis Adoption: Static Analysis Rule Sets Declared Exclusively"
metadata:
  actual:
    adr_id: 7976d9f5-74ef-49dc-abfb-759f0b0dcee1
    status: accepted
    candidate_group_id: 03d03f36-df4f-4d82-b61c-15651f701d85
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - "**"
      - "!apps/checker/**"
      - "!apps/dashboard/**"
      - "!apps/private-location/**"
      - "!apps/railway-proxy/**"
      - "!apps/screenshot-service/**"
      - "!apps/server/**"
      - "!apps/ssh-server/**"
      - "!apps/status-page/**"
      - "!apps/web/**"
      - "!apps/workflows/**"
      - "!packages/ai/**"
      - "!packages/analytics/**"
      - "!packages/api/**"
      - "!packages/assertions/**"
      - "!packages/db/**"
      - "!packages/emails/**"
      - "!packages/error/**"
      - "!packages/header-analysis/**"
      - "!packages/icons/**"
      - "!packages/importers/**"
      - "!packages/locales/**"
      - "!packages/notifications/base/**"
      - "!packages/notifications/bird-whatsapp/**"
      - "!packages/notifications/discord/**"
      - "!packages/notifications/email/**"
      - "!packages/notifications/google-chat/**"
      - "!packages/notifications/grafana-oncall/**"
      - "!packages/notifications/ms-teams/**"
      - "!packages/notifications/ntfy/**"
      - "!packages/notifications/opsgenie/**"
      - "!packages/notifications/pagerduty/**"
      - "!packages/notifications/slack/**"
      - "!packages/notifications/telegram/**"
      - "!packages/notifications/twillio-sms/**"
      - "!packages/notifications/webhook/**"
      - "!packages/proto/**"
      - "!packages/react/**"
      - "!packages/regions/**"
      - "!packages/services/**"
      - "!packages/status-fetcher/**"
      - "!packages/subscriptions/**"
      - "!packages/test-utils/**"
      - "!packages/theme-store/**"
      - "!packages/tinybird/**"
      - "!packages/tracker/**"
      - "!packages/tsconfig/**"
      - "!packages/ui/**"
      - "!packages/upstash/**"
      - "!packages/utils/**"
---

# Oxlint Static Code Analysis Adoption: Static Analysis Rule Sets Declared Exclusively

Scope: applies to the whole repository; excludes `!apps/checker/**`, `!apps/dashboard/**`, `!apps/private-location/**`, `!apps/railway-proxy/**`, `!apps/screenshot-service/**`, `!apps/server/**`, `!apps/ssh-server/**`, `!apps/status-page/**`, `!apps/web/**`, `!apps/workflows/**`, `!packages/ai/**`, `!packages/analytics/**`, `!packages/api/**`, `!packages/assertions/**`, `!packages/db/**`, `!packages/emails/**`, `!packages/error/**`, `!packages/header-analysis/**`, `!packages/icons/**`, `!packages/importers/**`, `!packages/locales/**`, `!packages/notifications/base/**`, `!packages/notifications/bird-whatsapp/**`, `!packages/notifications/discord/**`, `!packages/notifications/email/**`, `!packages/notifications/google-chat/**`, `!packages/notifications/grafana-oncall/**`, `!packages/notifications/ms-teams/**`, `!packages/notifications/ntfy/**`, `!packages/notifications/opsgenie/**`, `!packages/notifications/pagerduty/**`, `!packages/notifications/slack/**`, `!packages/notifications/telegram/**`, `!packages/notifications/twillio-sms/**`, `!packages/notifications/webhook/**`, `!packages/proto/**`, `!packages/react/**`, `!packages/regions/**`, `!packages/services/**`, `!packages/status-fetcher/**`, `!packages/subscriptions/**`, `!packages/test-utils/**`, `!packages/theme-store/**`, `!packages/tinybird/**`, `!packages/tracker/**`, `!packages/tsconfig/**`, `!packages/ui/**`, `!packages/upstash/**`, `!packages/utils/**`.

These rules are ALWAYS ACTIVE for all source files and modules managed within the repository subject to static analysis, excluding generated build artifacts, distribution bundles, and third-party vendor directories.

### Rules

- **R-SAC-001** MUST: Static analysis rule sets MUST be declared exclusively within the centralized repository configuration module rather than through ad-hoc, inline file overrides unless explicitly justified.

### Verify

```bash
# Discover the static analysis verification script from the repository manifest and execute it against all source files.
# Inspect the continuous integration pipeline configuration to locate the linting check target and validate execution.
```

**Accept when:**
- Static analysis completes across all in-scope source files with zero diagnostic errors.
- The static analysis configuration module is validated by the execution engine without syntax or schema warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>