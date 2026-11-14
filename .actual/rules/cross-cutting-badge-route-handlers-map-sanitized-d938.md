---
version: 2
name: cross-cutting-badge-route-handlers-map-sanitized-d938
description: "Adoption of @openstatus/react for Status Badge Rendering: Badge Route Handlers Map Sanitized Url"
metadata:
  actual:
    adr_id: d938ae14-4c77-4c1a-b960-1d11582197bb
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Badge Route Handlers Map Sanitized Url

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for public status page badge route handlers responsible for generating dynamic badge responses, and server-side endpoints rendering status indicator graphics from incoming query parameters.

### Rules

- **R-BADGE-001** SHOULD: Badge route handlers SHOULD map sanitized URL search parameters directly to component properties supported by @openstatus/react.
- **R-BADGE-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-BADGE-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation for that exact version. (5) Confirm every API, class, or function called exists in that version's documentation. (6) Re-run steps 3-5 per dependency at point of use.
- **R-BADGE-004** MANDATORY: Ensure request search parameter parsing extracts theme, size, and variant properties safely before passing them to badge components.
- **R-BADGE-005** MANDATORY: Encapsulate fallback default values for missing or invalid query parameters to ensure predictable badge output.

### Verify

```bash
# Find and execute the repository validation script defined in the project configuration to confirm type checking and build integrity for badge route handlers.
# Run the test suite discovered from the package configuration to verify badge route handler responses and parameter resolution.
```

**Accept when:**
- The repository test suite passes for all status badge route endpoints without regressions.
- All badge rendering routes resolve components exclusively from @openstatus/react without local styling duplication.
- Dependency resolution confirms that the installed version of @openstatus/react matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated dependency and type verification and peer code review enforce these rules. Violations require refactoring prior to approval, and exceptions require explicit core engineering team approval.
</enforcement>