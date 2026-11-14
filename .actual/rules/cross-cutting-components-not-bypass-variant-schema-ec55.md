---
version: 2
name: cross-cutting-components-not-bypass-variant-schema-ec55
description: "Adoption of class-variance-authority for Component Variant Styling: Components Not Bypass Variant Schema Apply"
metadata:
  actual:
    adr_id: ec554cc5-5a11-42c5-aa10-5e4ab6c2ee7b
    status: accepted
    candidate_group_id: a937ff75-96f0-4a05-a5bd-101569899b32
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Components Not Bypass Variant Schema Apply

Scope: applies to `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CVA-001** MUST_NOT: Components MUST NOT bypass the variant schema to apply conditional base variant overrides directly through unmanaged string operations.

### Verify

```bash
# Discover and execute the project test runner script to validate variant rendering.
# Discover and execute the project static analysis script to verify component styling conforms to standard variant declarations.
```

**Accept when:**
- All component test suites pass verifying correct styling application across all declared variant combinations.
- Static analysis and type checking complete with zero errors related to variant schema configurations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>