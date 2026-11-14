---
version: 2
name: cross-cutting-asynchronous-workflows-request-handlers-9a29
description: "@logtape/logtape Structured Logging Library: Asynchronous Workflows Request Handlers Requiring Ambient"
metadata:
  actual:
    adr_id: 9a299fb9-2ab7-4083-937c-6b2b4d189627
    status: accepted
    candidate_group_id: aff3c2d6-39b7-48ae-8760-0f8041099307
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# @logtape/logtape Structured Logging Library: Asynchronous Workflows Request Handlers Requiring Ambient

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-LOG-001** MUST: Asynchronous workflows and request handlers requiring ambient execution metadata MUST wrap execution scopes using withContext to preserve trace and request context.

### Verify

```bash
# Discover the repository static analysis script from the dependency manifest and execute it to verify that all logger imports reference @logtape/logtape.
# Discover and execute the project test runner script to validate that structured logging and context propagation assertions pass.
```

**Accept when:**
- Static analysis checks confirm that logging operations across all service modules import from @logtape/logtape and emit structured metadata objects.
- Automated test suites complete successfully and verify that contextual logging and error formatting comply with structured logging constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>