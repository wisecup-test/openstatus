---
version: 2
name: cross-cutting-consumer-discover-project-dependency-man-ba72
description: "@logtape/logtape Structured Logging Library: Consumer Discover Project Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: ba72f6a6-33bf-4f22-a4f8-bc4edb3d779b
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Consumer Discover Project Dependency Manifest Inspect

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all service handlers, cron tasks, health checkers, and background workflows within the service execution boundaries.

### Rules

- **R-LOG-001** MUST: The consumer MUST discover the project dependency manifest and inspect the resolution artifact to resolve and verify the exact locked version of @logtape/logtape prior to implementation.

### Verify

```bash
# Discover and execute the project static analysis script to verify logger imports
# Discover and execute the project test runner script to validate structured logging assertions
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>