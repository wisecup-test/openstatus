---
version: 2
name: cross-cutting-modules-performing-temporal-formatting-a-eaaf
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Modules Performing Temporal Formatting Alongside Utc"
metadata:
  actual:
    adr_id: eaaf7cab-cdb4-4d76-bd83-f9f50fad6607
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Modules Performing Temporal Formatting Alongside Utc

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for modules performing temporal formatting alongside UTC conversions, including status monitoring blocks, timestamp presentation components, and temporal formatting utility functions.

### Rules

- **R-UTC-001** SHOULD: Modules performing temporal formatting alongside UTC conversions integrate @date-fns/utc companion utilities with date-fns formatters rather than creating custom timezone offset arithmetic.
- **R-UTC-002** MANDATORY: Consumers derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-UTC-003** MANDATORY: Execute Lock-Version Grounding in order (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact version, confirm API existence, re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-UTC-004** MANDATORY: Construct status timestamp dates using UTC wrappers provided by @date-fns/utc before passing them into formatting utilities or UI presentation hooks.
- **R-UTC-005** MANDATORY: Ensure all shared formatting utility functions expose UTC-normalized interfaces to downstream status block components.

### Verify

```bash
# Discover the project test execution script from the repository manifest and execute it against status block utilities to verify UTC timestamp calculations.
# Discover the project static analysis and linting script from the repository manifest and execute it to verify dependency imports and type compliance across UI components.
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>