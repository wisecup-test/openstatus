---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-c-4878
description: "Adoption of @openstatus/db for Data Access and Schema Definition: Consumers Inspect Workspace Dependency Configuration Project"
metadata:
  actual:
    adr_id: 4878c20e-847b-48d9-95be-d81a78ee6b99
    status: accepted
    candidate_group_id: da7123d9-0b29-4b39-b7d6-dfa708206c4f
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
      - src/incident/**
      - src/lib/**
      - src/scripts/**
---

# Adoption of @openstatus/db for Data Access and Schema Definition: Consumers Inspect Workspace Dependency Configuration Project

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`.

These rules are ALWAYS ACTIVE for background workflow handlers, scheduled tasks, and operational utility routines requiring relational database persistence or entity schema validation.

### Rules

- **R-DB-001** MUST: Consumers MUST inspect the workspace dependency configuration and project lock artifact to resolve the exact dependency versions and schema definitions before implementing database access logic.
- **R-DB-002** MUST: Execute lock-version grounding in order: find dependency manifest, identify build tool, inspect lock artifact for exact resolved version, look up official documentation for that exact version, and confirm every API/class/function exists before use.
- **R-DB-003** MUST: Import query builder instances and entity schemas directly from the shared database package rather than initializing distinct database connections.
- **R-DB-004** MUST: Apply schema validation guards prior to persisting mutated status or incident records to ensure database constraints are respected.

### Verify

```bash
# Discover and execute the repository typecheck script from the project manifest
# Discover and execute the repository test runner covering background workflow jobs and database operations
```

**Accept when:**
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All persistent data interactions must import from the designated shared package, and automated verification checks executing static type inspection and project test suites must pass.
</enforcement>