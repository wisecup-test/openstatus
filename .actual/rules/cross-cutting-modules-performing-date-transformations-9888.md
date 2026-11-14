---
version: 2
name: cross-cutting-modules-performing-date-transformations-9888
description: "Adoption of date-fns for Functional Date and Time Manipulation: Modules Performing Date Transformations Import Individual"
metadata:
  actual:
    adr_id: 98887cad-1a79-4542-95f5-415b465b2de7
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

# Adoption of date-fns for Functional Date and Time Manipulation: Modules Performing Date Transformations Import Individual

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-DATE-001** MUST: Modules performing date transformations MUST import individual functional primitives directly from date-fns rather than implementing custom date arithmetic.
- **R-DATE-002** MUST: Follow the lock-version grounding process before writing code that uses a versioned library (discover manifest, identify build tool, inspect lock artifact for exact resolved version, fetch version-specific docs, and confirm API existence).
- **R-DATE-003** MUST: Import only the specific functional utilities required for the operation to preserve modularity.
- **R-DATE-004** MUST: Centralize recurring composite date operations, such as status window calculations and localized presentation formats, in shared utility modules.

### Verify

```bash
# Discover the project dependency manifest and lock artifact, identify the project verification script, and execute it to confirm dependency resolution and module integrity.
manifest_file=$(find . -maxdepth 2 -name "package.json" -o -name "pnpm-lock.yaml" -o -name "yarn.lock" -o -name "package-lock.json" | head -n 1)
echo "Found dependency artifacts:"
find . -name "package.json" -o -name "*-lock.*" 

# Run linting and static analysis to ensure all date transformations comply with import standards
if [ -f "package.json" ]; then
  npm run --if-present lint || npx eslint .
fi
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes involving temporal calculations and utility formatting are verified via automated linting rules, static type analysis, module boundary checks, and peer review. Pull requests introducing unapproved date libraries or direct mutable date arithmetic are flagged and blocked.
</enforcement>