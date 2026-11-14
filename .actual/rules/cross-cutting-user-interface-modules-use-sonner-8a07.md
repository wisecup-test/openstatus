---
version: 2
name: cross-cutting-user-interface-modules-use-sonner-8a07
description: "Adoption of sonner for Toast Notification Management: User Interface Modules Use Sonner Standardized"
metadata:
  actual:
    adr_id: 8a075af6-3cc0-47a3-a01d-452a912412f7
    status: accepted
    candidate_group_id: 9abf003d-098e-4232-bd99-6521c772f65f
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
      - src/hooks/**
---

# Adoption of sonner for Toast Notification Management: User Interface Modules Use Sonner Standardized

Scope: applies to `packages/ui/src/components/ui/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SONNER-001** MUST: The user interface modules MUST use sonner as the standardized library for rendering and dispatching transient toast notifications across user-facing interfaces.
- **R-SONNER-002** MANDATORY: Execute lock-version grounding prior to writing code that uses a versioned library by locating the manifest, identifying the build tool, inspecting the lock or resolution artifact for the exact resolved version, looking up official documentation for that version, and confirming every API/class/function exists in that version.
- **R-SONNER-003** MANDATORY: Mount the main toast wrapper component within the top-level application layout tree to ensure toasts render across all view routes.

### Verify

```bash
# Discover and run the repository test suite to validate toast notification rendering and interaction hook behavior.
# Discover and run the workspace static analysis and linting checks to ensure compliance with client boundary directives.
```

**Accept when:**
- All toast notification components render without runtime errors under client execution boundaries.
- Interactive hooks successfully dispatch toast notifications using sonner API bindings without regressions.
- Static analysis and test suites pass cleanly across all UI package modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated CI checks and peer code reviews. Violations will block pull requests.
</enforcement>