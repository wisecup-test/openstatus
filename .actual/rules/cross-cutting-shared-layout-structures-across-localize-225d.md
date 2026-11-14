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

These rules are ALWAYS ACTIVE for all public web application route layouts and navigation wrappers within the status page application service, and all domain- and locale-parameterized route segment hierarchies requiring layout composition and alternate metadata.

### Rules

- **R-NXT-001** SHOULD: Shared layout structures across localized route trees SHOULD be co-located within common parent route segments to maximize layout reusability and minimize redundant metadata computation.
- **R-NXT-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NXT-003** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact, (4) Look up official documentation/changelog for exact version, (5) Confirm every API/class/function exists in that exact version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-NXT-004** MANDATORY: Route layout components MUST export conforming layout signatures and integrate with the shared alternates metadata module for locale-aware canonical headers.
- **R-NXT-005** MANDATORY: Client-side navigation and routing hooks MUST be imported from the framework-native routing module in compliance with framework routing boundaries.

### Verify

```bash
# Discover the repository package manifest and execute the configured type checking and linting tasks
# Run the project automated test suite to verify route layout rendering, navigation behavior, and alternate metadata derivation across localized routes
```

**Accept when:**
- All route layout components import framework routing and navigation modules in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds and code reviews enforce adherence to framework routing mechanisms and metadata integration.
</enforcement>