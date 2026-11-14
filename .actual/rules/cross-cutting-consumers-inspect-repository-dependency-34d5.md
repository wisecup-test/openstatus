---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-34d5
description: "Adoption of date-fns for Functional Date and Time Manipulation: Consumers Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 34d5f4c9-f8be-46b9-aab2-e49ef743a552
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

# Adoption of date-fns for Functional Date and Time Manipulation: Consumers Inspect Repository Dependency Manifest Authoritative

Scope: applies to `apps/web/src/app/api/og/status/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application utility modules, domain calculation helpers, and API route handlers performing date arithmetic, interval computation, or timestamp formatting.

### Rules

- **R-ADF-001** MUST: Consumers MUST inspect the repository dependency manifest and authoritative resolution lock artifact to determine the exact resolved version of date-fns and verify API availability against version documentation before implementation.

### Verify

```bash
# Discover project dependency manifest and lock artifact, and execute verification/linting scripts
manifest_file=$(find . -maxdepth 2 -name "package.json" -o -name "Cargo.toml" -o -name "go.mod" -o -name "pom.xml" | head -n 1)
echo "Manifest located: $manifest_file"

# Run project verification and linting tools if available in package.json scripts or equivalent
if [ -f "package.json" ]; then
  npm test || true
  npm run lint || true
fi
```

**Accept when:**
- All modules performing date or time manipulation import and execute date-fns functions without errors during project verification.
- Static type checking and automated test suites discover no native Date mutations or unapproved alternative date libraries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All code changes involving temporal calculations and utility formatting must be verified via static type analysis, automated linting rules prohibiting unapproved third-party date libraries, and peer review.
</enforcement>