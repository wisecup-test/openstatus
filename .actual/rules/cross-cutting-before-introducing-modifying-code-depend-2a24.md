---
version: 2
name: cross-cutting-before-introducing-modifying-code-depend-2a24
description: "Gin Framework Service Boundary Definition and Context Propagation: Before Introducing Modifying Code Depending External"
metadata:
  actual:
    adr_id: 2a242771-005a-4bd6-bd04-45df97355e67
    status: accepted
    candidate_group_id: 35868210-db45-458b-9030-71b77645c31c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/server/**
      - handlers/**
      - pkg/otel/**
      - pkg/tinybird/**
---

# Gin Framework Service Boundary Definition and Context Propagation: Before Introducing Modifying Code Depending External

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Before introducing or modifying code depending on external libraries, the engineering team MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify API compatibility against official documentation.

### Verify

```bash
# 1. Find the dependency manifest and identify build tool
# 2. Inspect the repository lock or resolution artifact for exact resolved versions
# 3. Verify APIs against official documentation
# 4. Run test suites covering API boundary handlers using the project's configured test runner
# 5. Execute repository static analysis and linting tools
```

**Accept when:**
- All service boundary routes compile and pass automated test suites covering HTTP and protocol checking endpoints.
- Context metadata propagation operations complete without runtime type assertion failures or missing key errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated CI checks and mandatory peer code review.
</enforcement>