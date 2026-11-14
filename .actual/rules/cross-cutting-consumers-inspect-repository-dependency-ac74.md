---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-ac74
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Consumers Inspect Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: ac74d7a8-96e5-4ada-9473-dfb2cff35153
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Consumers Inspect Repository Dependency Manifest Resolution

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TB-001** MUST: Consumers MUST inspect the repository dependency manifest and resolution lock artifact to verify the exact resolved version of @openstatus/tinybird before introducing new integration calls.

### Verify

```bash
# Discover and execute project test runner for service context and monitor modules
# Discover and execute workspace static analysis and type-checking scripts
```

**Accept when:**
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated rejection of pull requests that instantiate unmanaged analytics connections or bypass centralized context configuration.
</enforcement>