---
version: 2
name: cross-cutting-embedded-assets-not-contain-dynamic-70e1
description: "embed Module Adoption: Embedded Assets Not Contain Dynamic Credentials"
metadata:
  actual:
    adr_id: 70e13363-7bc3-42be-bf68-233f631a96dc
    status: accepted
    candidate_group_id: 2530dec0-f9bd-4f1b-857d-16466d46cffc
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# embed Module Adoption: Embedded Assets Not Contain Dynamic Credentials

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for standalone services that compile static assets directly into binaries and components requiring immutable embedded file systems.

### Rules

- **R-EMB-001** MUST_NOT: Embedded assets MUST NOT contain dynamic credentials or secrets that require operational rotation outside the release cycle.

### Verify

```bash
# Discover and run the project's standard build and test suite scripts from the repository configuration to confirm asset compilation.
# Execute repository static analysis and security scanning checks discovered from the workspace configuration.
```

**Accept when:**
- The service compiles successfully with all embedded assets verified by automated tests.
- No relative filesystem path dependencies remain for bundled static assets.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>