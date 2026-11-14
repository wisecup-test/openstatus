---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-637d
description: "Adoption of next/navigation for Route Control in Server Handlers: Consumers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 637df91b-a222-4772-bd05-32d0aef40562
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Consumers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: Consumers MUST inspect the repository lock artifact to determine the exact resolved version of the framework navigation library and verify API compatibility against official version-specific documentation before implementing or modifying navigation call sites.

### Verify

```bash
# Discover and execute the project's static analysis and type checking script
# Discover and execute the project's automated test suite
```

**Accept when:**
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>