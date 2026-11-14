---
version: 2
name: cross-cutting-production-application-modules-not-repli-fd31
description: "Direct Process Environment Variable Access via Process Module: Production Application Modules Not Replicate Direct"
metadata:
  actual:
    adr_id: fd312ef4-696d-4a8e-91e4-61f6b22fea5b
    status: accepted
    candidate_group_id: 641a54a0-56d3-41a2-a284-dcbd11067dc6
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Direct Process Environment Variable Access via Process Module: Production Application Modules Not Replicate Direct

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build automation scripts, transformation scripts, and local development execution contexts requiring runtime deployment metadata.

### Rules

- **R-ENV-001** MUST_NOT: Production application modules MUST NOT replicate direct unvalidated access to the process environment and SHOULD isolate configuration ingestion within dedicated boundary modules.

### Verify

```bash
# Discover and run repository verification scripts to confirm environment configuration parsing complies with project standards
# Execute repository linting suites to ensure process environment access remains restricted to authorized utility scripts
```

**Accept when:**
- Repository verification scripts pass without reporting unauthorized process environment access.
- Utility scripts successfully parse required platform environment variables and exit cleanly.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>