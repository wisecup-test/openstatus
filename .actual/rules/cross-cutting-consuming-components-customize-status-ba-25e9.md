---
version: 2
name: cross-cutting-consuming-components-customize-status-ba-25e9
description: "Adoption of @openstatus/ui Status Block Components and Shared Types: Consuming Components Customize Status Bar Item"
metadata:
  actual:
    adr_id: 25e9cf1d-f142-4943-847d-4ff68018a99c
    status: accepted
    candidate_group_id: 6ffc73a3-906b-49a6-bf40-eca1e97f23d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/ui Status Block Components and Shared Types: Consuming Components Customize Status Bar Item

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for status dashboard pages, incident timeline views, service history displays, and data aggregation functions that map telemetry and incident records into daily status intervals.

### Rules

- **R-OPENSTATUS-001** MAY: Consuming components MAY customize status bar item appearance using supported component slot selectors and utility classes when specific layout variations are required.
- **R-OPENSTATUS-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-OPENSTATUS-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at the point of use for version-sensitive behavior.
- **R-OPENSTATUS-004** MANDATORY: Import status types and status bar components using canonical module resolution paths provided by the UI package.
- **R-OPENSTATUS-005** MANDATORY: Ensure client components isolate stateful interactivity while maintaining purity in underlying status aggregation functions.

### Verify

```bash
# Discover and execute the repository type-checking script across all workspace packages
# (Derive exact command from project repository package.json/scripts)
npm run typecheck

# Discover and run the repository linter to detect unapproved local status type definitions
npm run lint

# Execute the automated test suite
npm test
```

**Accept when:**
- All components rendering status timelines import presentation blocks and type contracts exclusively from @openstatus/ui.
- Static type checking and automated test suites confirm zero type mismatches across status calculations and UI bindings.
- Workspace verification pipelines pass with zero unresolved dependencies or unregistered module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated type checking, linting, and peer code review strictly enforce adherence to shared component contracts and prohibit ad-hoc status type definitions.
</enforcement>