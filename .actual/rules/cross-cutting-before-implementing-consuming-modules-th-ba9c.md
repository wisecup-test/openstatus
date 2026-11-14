---
version: 2
name: cross-cutting-before-implementing-consuming-modules-th-ba9c
description: "Internal env Module Adoption for Centralized Environment Configuration: Before Implementing Consuming Modules That Interface"
metadata:
  actual:
    adr_id: ba9c15ff-5913-4abd-9302-6662ff3c9b00
    status: accepted
    candidate_group_id: 7c1d83aa-a233-4b90-af47-a3f448078481
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/incident/**
      - src/lib/**
      - src/scripts/**
      - src/utils/**
---

# Internal env Module Adoption for Centralized Environment Configuration: Before Implementing Consuming Modules That Interface

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow handlers, cron routines, HTTP route handlers, infrastructure client adapters, and internal service scripts requiring runtime configuration.

### Rules

- **R-ENV-001** MUST: Before implementing or consuming modules that interface with external packages or runtime environments, the consumer MUST inspect the repository lock or resolution artifact to determine the exact resolved dependency versions.
- **R-ENV-002** MUST: Access all runtime configuration, credentials, and endpoints exclusively through the centralized internal env module using the `env()` accessor function.
- **R-ENV-003** MUST: Ensure newly introduced configuration keys are added to the internal configuration definition before referencing them in workflow handlers or middleware.

### Verify

```bash
# Discover the project task runner or verification scripts and execute static analysis checks
if [ -f package.json ]; then
  npm run lint || npx eslint .
fi

# Execute the project test suite to verify configuration retrieval and service initialization
if [ -f package.json ]; then
  npm test
fi
```

**Accept when:**
- Static analysis confirms all environment access within workflow and background modules routes exclusively through the internal env module.
- Automated test executions pass with validated environment configurations across all service routes and cron tasks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>