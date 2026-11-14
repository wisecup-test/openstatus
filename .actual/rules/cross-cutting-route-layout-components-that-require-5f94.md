---
version: 2
name: cross-cutting-route-layout-components-that-require-5f94
description: "Adoption of next Framework for Route Layouts and Navigation Architecture: Route Layout Components That Require Client"
metadata:
  actual:
    adr_id: 5f947366-12a0-4119-a017-68f88fea98f0
    status: accepted
    candidate_group_id: 04208163-6b55-4975-9444-52819ae6503a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next Framework for Route Layouts and Navigation Architecture: Route Layout Components That Require Client

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all public web application route layouts and navigation wrappers within the status page application service, and all domain- and locale-parameterized route segment hierarchies requiring layout composition and alternate metadata.

### Rules

- **R-NAV-001** MUST: Route layout components that require client-side routing, URL manipulation, or navigation context MUST derive navigation primitives exclusively from next/navigation.

### Verify

```bash
# Discover the repository package manifest and execute the configured type checking and linting tasks to validate layout component interfaces and import paths.
# Run the project automated test suite to verify route layout rendering, navigation behavior, and alternate metadata derivation across localized routes.
```

**Accept when:**
- All route layout components import next and next/navigation in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, type checking, and peer code review.
</enforcement>