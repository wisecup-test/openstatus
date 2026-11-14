---
version: 2
name: cross-cutting-developers-not-embed-raw-inline-40ef
description: "Adoption of lucide-react for Dashboard Iconography: Developers Not Embed Raw Inline Svg"
metadata:
  actual:
    adr_id: 40efbcb3-79dd-4099-87ae-c86a5e25e5b0
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

# Adoption of lucide-react for Dashboard Iconography: Developers Not Embed Raw Inline Svg

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LUCIDE-001** MUST_NOT: Developers MUST NOT embed raw inline SVG icon paths or ad-hoc custom vector markup within component files when an equivalent icon exists within lucide-react.

### Verify

```bash
# Discover and execute the project type-checking and component test scripts defined in the repository manifest.
# Run the project static analysis and linting suites to ensure no unapproved inline SVG assets or rogue icon libraries are imported.
```

**Accept when:**
- All component views render visual icons through named imports from lucide-react without unresolved import errors.
- Project static analysis and type verification suites pass with zero errors across all affected component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. All component additions and modifications must be verified via automated static analysis, lint rules, and peer code review.
</enforcement>