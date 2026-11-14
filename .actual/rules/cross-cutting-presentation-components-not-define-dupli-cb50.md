---
version: 2
name: cross-cutting-presentation-components-not-define-dupli-cb50
description: "Adoption of @/lib/utils Internal Utility Module for Presentation Components: Presentation Components Not Define Duplicated Helper"
metadata:
  actual:
    adr_id: cb507501-4bc4-4f5c-b1a0-8ba8aaddc5bb
    status: accepted
    candidate_group_id: f3dc5d23-ec5a-453d-880d-3f740a5b058d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @/lib/utils Internal Utility Module for Presentation Components: Presentation Components Not Define Duplicated Helper

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST_NOT: Presentation components MUST NOT define duplicated helper logic inline when equivalent operations are provided by the @/lib/utils module.

### Verify

```bash
# Discover and execute the repository type-checking script
# Discover and execute the repository test suite
# Discover and execute the repository linter
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all components importing the utility module.
- All automated unit and regression tests for utility helpers and consuming components pass without failures.
- Linting checks confirm no duplicate local utility implementations or forbidden relative paths exist.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration validation pipelines and peer code reviews enforce compliance.
</enforcement>