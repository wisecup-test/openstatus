---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-2382
description: "Adoption of @openstatus/react for Status Badge Rendering: Consumers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 23828f58-027d-4fd2-8aab-0a2ea47e3f66
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Consumers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for public status page badge route handlers and server-side endpoints rendering status indicator graphics.

### Rules

- **R-OPENSTATUS-001** MUST: Consumers MUST inspect the repository lock artifact to determine the exact resolved version of @openstatus/react before implementation and verify API compatibility against the official documentation for that exact version.
- **R-OPENSTATUS-002** MUST: Ensure request search parameter parsing extracts theme, size, and variant properties safely before passing them to badge components.
- **R-OPENSTATUS-003** MUST: Encapsulate fallback default values for missing or invalid query parameters to ensure predictable badge output.

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
Claude Code MUST NOT skip or defer verification. Automated dependency/type verification and peer code review are strictly enforced.
</enforcement>