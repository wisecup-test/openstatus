---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-cc97
description: "Centralized Route Resolution via resolve-route Module: Consumers Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: cc97a80b-fece-40ea-b37a-b9b191dcf54d
    status: accepted
    candidate_group_id: c1192f46-e2dc-4500-93bd-53907a2f23e6
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Centralized Route Resolution via resolve-route Module: Consumers Inspect Repository Dependency Manifest Authoritative

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CRR-001** MUST: Consumers MUST inspect the repository dependency manifest and authoritative lock artifact to verify workspace resolution integrity before modifying routing dependencies.

### Verify

```bash
# Discover and execute workspace static analysis and linting scripts
# Discover and run project type checking and test suite scripts
```

**Accept when:**
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks and peer code reviews.
</enforcement>