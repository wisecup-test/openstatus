---
version: 2
name: cross-cutting-consumers-discover-project-dependency-re-fd66
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Consumers Discover Project Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: fd66e517-0717-4831-a26c-55752762ffae
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Consumers Discover Project Dependency Resolution Artifact

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all service packages requiring database queries, schema models, or entity state persistence, and event dispatch and notification channel modules managing subscriber records and status updates.

### Rules

- **R-ADR-001** MUST: Consumers MUST discover the project dependency resolution artifact and verify the exact resolved version of internal and external dependencies prior to implementation.

### Verify

\`\`\`bash
# Discover and run the repository static analysis and linting scripts to verify persistence imports originate exclusively from the authorized shared database module.
npx turbo run lint

# Discover and execute the automated test suite to confirm database query and mutation operations fulfill schema and ordering contracts.
npx turbo run test

# Execute the project type-checking script to ensure all schema entities and query parameter objects conform to shared database module definitions.
npx turbo run typecheck
\`\`\`

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>