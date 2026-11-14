---
version: 2
name: cross-cutting-before-integrating-updating-any-icon-4f52
description: "Adoption of lucide-react for Dashboard Iconography: Before Integrating Updating Any Icon Import"
metadata:
  actual:
    adr_id: 4f521889-8b79-41b1-91ce-d1722b587195
    status: accepted
    candidate_group_id: 5f56dc68-a638-4a7f-8036-e481d4a9289d
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
---

# Adoption of lucide-react for Dashboard Iconography: Before Integrating Updating Any Icon Import

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** MUST: Before integrating or updating any icon import from lucide-react, the consumer MUST inspect the repository dependency lock artifact to determine the exact resolved version and verify that imported icon identifiers exist in that version reference.

### Verify

```bash
# Discover and execute the project type-checking and component test scripts defined in the repository manifest.
# Run the project static analysis and linting suites to ensure no unapproved inline SVG assets or rogue icon libraries are imported.
```

**Accept when:**
- All component views render visual icons through named imports from lucide-react without unresolved import errors.
- Project static analysis and type verification suites pass with zero errors across all affected component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis and lint rules in continuous integration validating approved icon import sources, and peer code review for all frontend component additions and modifications.
</enforcement>