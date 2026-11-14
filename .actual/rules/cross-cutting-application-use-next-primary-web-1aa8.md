---
version: 2
name: cross-cutting-application-use-next-primary-web-1aa8
description: "Adoption of next Framework for Route Layouts and Navigation Architecture: Application Use Next Primary Web Framework"
metadata:
  actual:
    adr_id: 1aa8d8e8-71c1-4910-80ac-17fe6ef3d9ad
    status: accepted
    candidate_group_id: 04208163-6b55-4975-9444-52819ae6503a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next Framework for Route Layouts and Navigation Architecture: Application Use Next Primary Web Framework

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NEXT-001** MUST: The application MUST use next as the primary web application framework for routing, hierarchical layout structuring, and navigation across all public application route segments.
- **R-NEXT-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NEXT-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding process in order (find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).
- **R-NEXT-004** MUST: Verify that all route layout components export conforming layout signatures and integrate with the shared alternates metadata module for locale-aware canonical headers.
- **R-NEXT-005** MUST: Ensure client-side navigation and routing hooks are imported from next/navigation in compliance with framework routing boundaries.

### Verify

```bash
# Discover repository package manifest and execute configured type checking and linting tasks
# (Discover the specific commands from the repository package.json scripts, e.g., npm run lint / npm run typecheck)

# Run the project automated test suite to verify route layout rendering, navigation behavior, and alternate metadata derivation
# (Discover the test command from the repository, e.g., npm test)
```

**Accept when:**
- All route layout components import next and next/navigation in accordance with framework routing conventions without type or lint errors.
- Localized route layouts successfully incorporate the centralized alternates metadata utility and generate valid alternate link tags during automated verification.
- Project verification and test scripts execute without route resolution or navigation contract failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>