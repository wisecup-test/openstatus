---
version: 2
name: cross-cutting-developers-not-define-localized-hoc-69a6
description: "Adoption of oxfmt for Code Formatting and Module Normalization: Developers Not Define Localized Hoc Formatting"
metadata:
  actual:
    adr_id: 69a6505c-cf7f-45e8-ad5a-945b67ee3301
    status: accepted
    candidate_group_id: 51a59959-4b9a-4c8b-af08-2ccfb27e6231
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

# Adoption of oxfmt for Code Formatting and Module Normalization: Developers Not Define Localized Hoc Formatting

Scope: applies to the whole repository; excludes `!apps/checker/**`, `!apps/dashboard/**`, `!apps/private-location/**`, `!apps/railway-proxy/**`, `!apps/screenshot-service/**`, `!apps/server/**`, `!apps/ssh-server/**`, `!apps/status-page/**`, `!apps/web/**`, `!apps/workflows/**`, `!packages/ai/**`, `!packages/analytics/**`, `!packages/api/**`, `!packages/assertions/**`, `!packages/db/**`, `!packages/emails/**`, `!packages/error/**`, `!packages/header-analysis/**`, `!packages/icons/**`, `!packages/importers/**`, `!packages/locales/**`, `!packages/notifications/base/**`, `!packages/notifications/bird-whatsapp/**`, `!packages/notifications/discord/**`, `!packages/notifications/email/**`, `!packages/notifications/google-chat/**`, `!packages/notifications/grafana-oncall/**`, `!packages/notifications/ms-teams/**`, `!packages/notifications/ntfy/**`, `!packages/notifications/opsgenie/**`, `!packages/notifications/pagerduty/**`, `!packages/notifications/slack/**`, `!packages/notifications/telegram/**`, `!packages/notifications/twillio-sms/**`, `!packages/notifications/webhook/**`, `!packages/proto/**`, `!packages/react/**`, `!packages/regions/**`, `!packages/services/**`, `!packages/status-fetcher/**`, `!packages/subscriptions/**`, `!packages/test-utils/**`, `!packages/theme-store/**`, `!packages/tinybird/**`, `!packages/tracker/**`, `!packages/tsconfig/**`, `!packages/ui/**`, `!packages/upstash/**`, `!packages/utils/**`.

These rules are ALWAYS ACTIVE for all first-party source code modules maintained within the repository, excluding third-party vendor libraries and auto-generated source artifacts designated in repository exclusion lists.

### Rules

- **R-FMT-001** MUST_NOT: Developers MUST NOT define localized or ad-hoc formatting configurations that conflict with or override the centralized oxfmt configuration.

### Verify

```bash
# Discover and execute the repository formatting verification script defined in the project manifest
# Run the project static analysis and formatting check command in non-modifying check mode
```

**Accept when:**
- The discovered formatting verification command completes with zero syntax or style violations and an exit code of zero.
- All committed source files match the formatting rules declared in the repository configuration artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and pre-merge reviews execute automated formatting checks to reject unformatted or non-compliant pull requests.
</enforcement>