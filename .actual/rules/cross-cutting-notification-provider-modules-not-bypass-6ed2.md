---
version: 2
name: cross-cutting-notification-provider-modules-not-bypass-6ed2
description: "Adoption of @openstatus/notification-base for Notification Integrations: Notification Provider Modules Not Bypass Openstatus"
metadata:
  actual:
    adr_id: 6ed2c23c-315a-40e3-b89a-08696c56ee85
    status: accepted
    candidate_group_id: 3f40d750-09fd-4476-b815-be9cf4a85d79
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Integrations: Notification Provider Modules Not Bypass Openstatus

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST_NOT: Notification provider modules MUST NOT bypass @openstatus/notification-base to define redundant, provider-specific notification contract abstractions.

### Verify

```bash
# Discover and run repository build, test, and lint validation scripts
npm run build || pnpm build || yarn build
npm test || pnpm test || yarn test
npm run lint || pnpm lint || yarn lint
```

**Accept when:**
- All notification provider modules successfully compile and satisfy interface contracts defined in @openstatus/notification-base.
- External dispatch execution tests pass with valid payload assertions and schema validation checks.
- Dependency boundaries and package resolution checks succeed without unresolved imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer architectural reviews enforce adherence to base library contracts, and pull requests bypassing shared base contracts or introducing unvalidated external client dispatches will be blocked from merge.
</enforcement>