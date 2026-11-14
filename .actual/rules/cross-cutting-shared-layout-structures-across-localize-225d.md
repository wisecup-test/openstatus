---
version: 2
name: cross-cutting-shared-layout-structures-across-localize-225d
description: "Adoption of next Framework for Route Layouts and Navigation Architecture: Shared Layout Structures Across Localized Route"
metadata:
  actual:
    adr_id: 225dd7eb-c05e-4d35-9a45-52089c6c936d
    status: accepted
    candidate_group_id: 04208163-6b55-4975-9444-52819ae6503a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next Framework for Route Layouts and Navigation Architecture: Shared Layout Structures Across Localized Route

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public web application route layouts and navigation wrappers within the status page application service and all domain- and locale-parameterized route segment hierarchies requiring layout composition and alternate metadata.

### Rules

- **R-LAYOUT-001** SHOULD: Shared layout structures across localized route trees SHOULD be co-located within common parent route segments to maximize layout reusability and minimize redundant metadata computation.

### Verify

```bash
# Discover the repository package manifest and execute the configured type checking and linting tasks
# Run the project automated test suite to verify route layout rendering, navigation behavior, and alternate metadata derivation
```

**Accept when:**
- All route layout components import framework routing modules and core libraries in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>