---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-20d5
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Engineers Discover Repository Dependency Manifest Corresponding"
metadata:
  actual:
    adr_id: 20d5eed2-87d0-496a-9d56-d653e9a4b2de
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Engineers Discover Repository Dependency Manifest Corresponding

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status monitoring blocks, timestamp presentation components, and temporal formatting utility modules.

### Rules

- **R-UTC-001** MUST: Engineers MUST discover the repository dependency manifest and corresponding lock or resolution artifact to determine the exact locked version of @date-fns/utc before implementing or updating date operations.

### Verify

```bash
# Discover the project test execution script from the repository manifest and execute it against status block utilities
# Discover the project static analysis and linting script from the repository manifest and execute it
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>