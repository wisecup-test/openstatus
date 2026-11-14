---
version: 2
name: cross-cutting-development-environments-configured-exec-5417
description: "Adoption of oxfmt for Code Formatting and Module Normalization: Development Environments Configured Execute Oxfmt Formatting"
metadata:
  actual:
    adr_id: 5417f757-d801-4e90-aa51-4ba5afbb6695
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

# Adoption of oxfmt for Code Formatting and Module Normalization: Development Environments Configured Execute Oxfmt Formatting

Scope: applies to the whole repository; excludes `!apps/checker/**`, `!apps/dashboard/**`, `!apps/private-location/**`, `!apps/railway-proxy/**`, `!apps/screenshot-service/**`, `!apps/server/**`, `!apps/ssh-server/**`, `!apps/status-page/**`, `!apps/web/**`, `!apps/workflows/**`, `!packages/ai/**`, `!packages/analytics/**`, `!packages/api/**`, `!packages/assertions/**`, `!packages/db/**`, `!packages/emails/**`, `!packages/error/**`, `!packages/header-analysis/**`, `!packages/icons/**`, `!packages/importers/**`, `!packages/locales/**`, `!packages/notifications/base/**`, `!packages/notifications/bird-whatsapp/**`, `!packages/notifications/discord/**`, `!packages/notifications/email/**`, `!packages/notifications/google-chat/**`, `!packages/notifications/grafana-oncall/**`, `!packages/notifications/ms-teams/**`, `!packages/notifications/ntfy/**`, `!packages/notifications/opsgenie/**`, `!packages/notifications/pagerduty/**`, `!packages/notifications/slack/**`, `!packages/notifications/telegram/**`, `!packages/notifications/twillio-sms/**`, `!packages/notifications/webhook/**`, `!packages/proto/**`, `!packages/react/**`, `!packages/regions/**`, `!packages/services/**`, `!packages/status-fetcher/**`, `!packages/subscriptions/**`, `!packages/test-utils/**`, `!packages/theme-store/**`, `!packages/tinybird/**`, `!packages/tracker/**`, `!packages/tsconfig/**`, `!packages/ui/**`, `!packages/upstash/**`, `!packages/utils/**`.

These rules are ALWAYS ACTIVE for all first-party source code modules maintained within the repository, and automated formatting and code style verification routines within local development and pipeline workflows.

### Rules

- **R-OX-001** SHOULD: Development environments SHOULD be configured to execute oxfmt formatting automatically upon file save to maintain continuous compliance.
- **R-OX-002** MANDATORY: The consumer MUST discover all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-OX-003** MANDATORY: Before writing code that uses a versioned library, the developer MUST execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact resolved version, (4) Look up official documentation for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover and execute the repository formatting verification script defined in the project manifest.
# Discover and run the project static analysis and formatting check command in non-modifying check mode.
```

**Accept when:**
- The discovered formatting verification command completes with zero syntax or style violations and an exit code of zero.
- All committed source files match the formatting rules declared in the repository configuration artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated checks reject unformatted or non-compliant pull requests, blocking integration until formatting violations are resolved. Developers must run the discovered repository formatting task to normalize affected files before merging.
</enforcement>