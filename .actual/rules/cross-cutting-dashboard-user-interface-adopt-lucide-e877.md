---
version: 2
name: cross-cutting-dashboard-user-interface-adopt-lucide-e877
description: "Adoption of lucide-react for Dashboard Iconography: Dashboard User Interface Adopt Lucide React"
metadata:
  actual:
    adr_id: e8777a42-73a2-45e2-8c8d-9e02221755e2
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

# Adoption of lucide-react for Dashboard Iconography: Dashboard User Interface Adopt Lucide React

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LUCIDE-001** MUST: The dashboard user interface MUST adopt lucide-react as the primary component library for visual iconography across interactive controls, navigation lists, and data table headers and pagination.
- **R-LUCIDE-002** MUST (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-LUCIDE-003** MUST (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool. (3) Inspect the repository lock or resolution artifact for the exact resolved version. (4) Look up official documentation for that exact version. (5) Confirm every API, class, or function used exists in that version. (6) Re-run steps 3-5 per dependency at point of use.
- **R-LUCIDE-004** MUST: Import only the required icon components using named imports to preserve tree-shaking efficiency.
- **R-LUCIDE-005** MUST: Apply standard visual styling such as height, width, and color classes directly to the icon component rather than wrapping it in redundant container nodes unless necessary for layout.

### Verify

```bash
# Discover and execute the project type-checking and component test scripts defined in the repository manifest.
# Run the project static analysis and linting suites to ensure no unapproved inline SVG assets or rogue icon libraries are imported.
```

**Accept when:**
- All component views render visual icons through named imports from lucide-react without unresolved import errors.
- Project static analysis and type verification suites pass with zero errors across all affected component files.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, lint rules, and peer code reviews enforce compliance, and pull requests introducing unapproved icon libraries or raw inline SVG markup without an approved exception are blocked until refactored.
</enforcement>