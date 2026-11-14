---
version: 2
name: cross-cutting-application-use-date-fns-standard-19c4
description: "Adoption of date-fns for Functional Date and Time Manipulation: Application Use Date Fns Standard Library"
metadata:
  actual:
    adr_id: 19c4c70d-909b-493d-b63a-1122c454fcf0
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

# Adoption of date-fns for Functional Date and Time Manipulation: Application Use Date Fns Standard Library

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-DATE-001** MUST: The application MUST use date-fns as the standard library for all date parsing, arithmetic, interval comparison, and formatting operations across shared utilities and API routes.
- **R-DATE-002** MUST: Follow the LOCK-VERSION GROUNDING procedure before writing code that uses a versioned library: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, and confirm every API/class/function exists before use.
- **R-DATE-003** MUST: Import only the specific functional utilities required for the operation to preserve modularity.
- **R-DATE-004** SHOULD: Centralize recurring composite date operations, such as status window calculations and localized presentation formats, in shared utility modules.

### Verify

```bash
# Discover dependency manifest and lock artifact, identify verification script, and execute it:
# (Example discovery & verification sequence derived from repository structure)
manifest=$(find . -maxdepth 2 -name "package.json" -o -name "Cargo.toml" -o -name "go.mod" | head -n 1)
echo "Found manifest: $manifest"

# Run project linting and static analysis commands to ensure import compliance
if [ -f "package.json" ]; then
  npm run lint || npx eslint .
fi
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated linting, static type analysis, module boundary checks, and peer review enforce adherence to date-fns standards, blocking unapproved date libraries or direct mutable date arithmetic.
</enforcement>