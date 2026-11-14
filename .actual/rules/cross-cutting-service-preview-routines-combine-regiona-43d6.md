---
version: 2
name: cross-cutting-service-preview-routines-combine-regiona-43d6
description: "Adoption of @openstatus/regions for Regional Monitoring Services: Service Preview Routines Combine Regional Headers"
metadata:
  actual:
    adr_id: 43d62a2c-b549-4af1-9ff3-f9f3ec5301ba
    status: accepted
    candidate_group_id: 7d6c947a-2ce9-4980-87e0-d71f41deed1f
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/regions for Regional Monitoring Services: Service Preview Routines Combine Regional Headers

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for all monitoring domain service handlers and preview streaming routines that interface with regional probe endpoints, internal database queries, and service workflows requiring geographic region metadata.

### Rules

- **R-OPENSTATUS-001** MAY: Service preview routines MAY combine regional headers with localized secret authorization headers when dispatching authenticated probe simulation requests.
- **R-OPENSTATUS-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-OPENSTATUS-003** MANDATORY: Execute the lock-version grounding process before writing code that uses a versioned library: find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for the exact resolved version, and confirm every API/class/function exists in that version.
- **R-OPENSTATUS-004** MANDATORY: Verify that all outgoing HTTP client dispatches targeting regional endpoints spread standardized regional headers derived from @openstatus/regions.
- **R-OPENSTATUS-005** MANDATORY: Configure regional dispatch client calls with abort signal timeouts calibrated to regional timeout constants exported by @openstatus/regions.

### Verify

```bash
# Discover and execute the project type-checking script from the repository manifest to verify @openstatus/regions import compliance across services.
# Discover and run the project test suite from the repository manifest to validate regional header injection and preview dispatch handling.
```

**Accept when:**
- Static analysis and type validation pass across all service modules importing @openstatus/regions without unresolved symbols or mismatched export contracts.
- Regional monitoring dispatch and stream preview tests pass, confirming standardized regional header propagation and timeout signal bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>