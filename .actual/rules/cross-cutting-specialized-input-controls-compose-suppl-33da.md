---
version: 2
name: cross-cutting-specialized-input-controls-compose-suppl-33da
description: "Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Specialized Input Controls Compose Supplementary Contextual"
metadata:
  actual:
    adr_id: 33da962a-2368-4a41-884a-d2c119efcc04
    status: accepted
    candidate_group_id: 9f73dff5-6372-4a30-8d58-fd257f35ff4e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Specialized Input Controls Compose Supplementary Contextual

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for client-rendered interactive user interface components requiring grouped input fields, action buttons, or inline adornments, and form interfaces and interactive prompt inputs within the dashboard application boundary.

### Rules

- **R-INPUT-001** MAY: Specialized input controls MAY compose supplementary contextual elements or action triggers within the input group structure as required by component interaction specifications.

### Verify

\`\`\`bash
# Discover and run the project repository type verification suite to validate module import paths and interface contracts.
# Discover and execute the repository linter and component validation scripts to check for compliance with design system import rules.
\`\`\`

**Accept when:**
- The repository type verification suite completes without errors regarding internal module imports.
- All composite input fields in client components resolve their layout structure through the designated input group module without local wrapper duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, module import linting, and peer code review verification apply.
</enforcement>