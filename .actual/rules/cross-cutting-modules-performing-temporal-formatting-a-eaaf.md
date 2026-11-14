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

These rules are ALWAYS ACTIVE for status monitoring blocks, timestamp presentation components, and temporal formatting utility functions.

### Rules

- **R-UTC-001** SHOULD: Modules performing temporal formatting alongside UTC conversions SHOULD integrate @date-fns/utc companion utilities with date-fns formatters rather than creating custom timezone offset arithmetic.
- **R-UTC-002** MANDATORY: Construct status timestamp dates using UTC wrappers provided by @date-fns/utc before passing them into formatting utilities or UI presentation hooks.
- **R-UTC-003** MANDATORY: Ensure all shared formatting utility functions expose UTC-normalized interfaces to downstream status block components.
- **R-UTC-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that version, and confirm every API/class/function exists before using it.

### Verify

```bash
# Discover and execute the project test execution script against status block utilities
# Discover and execute the project static analysis and linting script to verify dependency imports and type compliance
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipelines and peer code review. Pull requests introducing uncoordinated date instantiation or unapproved date libraries in status blocks are blocked pending refactoring.
</enforcement>