---
version: 2
name: cross-cutting-domain-specific-date-formatting-strings-1d56
description: "Adoption of date-fns for Functional Date and Time Manipulation: Domain Specific Date Formatting Strings Composite"
metadata:
  actual:
    adr_id: 1d5659f0-8f74-46ee-b4ec-004daa637e54
    status: accepted
    candidate_group_id: e914c52b-35bd-4db1-bc0f-89fb166ab748
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/status/**
      - src/lib/**
---

# Adoption of date-fns for Functional Date and Time Manipulation: Domain Specific Date Formatting Strings Composite

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-DATE-001** SHOULD: Domain-specific date formatting strings and composite temporal calculations SHOULD be encapsulated in centralized utility helpers rather than duplicated across individual endpoint routes.
- **R-DATE-002** MANDATORY: The consumer MUST discover the project dependency manifest, lock artifact, and verification/linting commands from the repository.
- **R-DATE-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding steps in order (manifest -> build tool -> lock artifact -> version-specific docs -> API confirmation).
- **R-DATE-004** MANDATORY: Import only the specific functional utilities required for the operation to preserve modularity, and centralize recurring composite date operations in shared utility modules.

### Verify

```bash
# Discover the project dependency manifest and lock artifact, identify the project verification script, and execute it:
# (Example discovery and verification flow derived from repository configuration)
if [ -f "package.json" ]; then
  npm test || pnpm test || yarn test
fi

# Inspect repository configuration to locate project linting and static analysis commands:
if [ -f "package.json" ]; then
  npm run lint || pnpm lint || yarn lint
fi
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated linting, static type analysis, and module boundary checks are enforced during continuous integration, and pull requests violating these standards are blocked.
</enforcement>