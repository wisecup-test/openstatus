---
version: 2
name: cross-cutting-before-introducing-upgrading-any-externa-c41e
description: "Console API Tagged Prefix Logging for Error and Warning Diagnostics: Before Introducing Upgrading Any External Observability"
metadata:
  actual:
    adr_id: c41e1393-c04a-4603-81f2-69891131ea44
    status: accepted
    candidate_group_id: 36d6268e-35a8-4f3e-88ed-9486d899106e
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
      - src/**
---

# Console API Tagged Prefix Logging for Error and Warning Diagnostics: Before Introducing Upgrading Any External Observability

Scope: applies to `packages/tinybird/scripts/**`, `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for client integration modules managing external analytics connectivity, safe query execution, and operational data backfill and migration scripts interacting with primary datastores.

### Rules

- **R-OBS-001** MUST: Before introducing or upgrading any external observability or logging dependency, inspect the repository lock artifact to determine the exact resolved dependency version.
- **R-OBS-002** MUST: Use standard Console API for operational error and warning logging in lightweight client integration packages to avoid heavy third-party logging dependencies.
- **R-OBS-003** MUST: Tag log statements with bracketed component prefixes to provide contextual attribution in container and serverless runtime log streams.
- **R-OBS-004** MUST: Log errors prior to returning empty fallback data to ensure operational transparency while preserving fault-tolerant execution.
- **R-OBS-005** MUST: Attach caught error objects as a secondary parameter rather than concatenating them as a string to preserve stack traces in safe fallback routines.
- **R-OBS-006** MUST: Lock version grounding by finding the dependency manifest, identifying the build tool, inspecting the lock or resolution artifact for the exact resolved version, looking up official docs for that version, and confirming every API/class/function exists in that exact version before writing code.

### Verify

```bash
# Discover and execute repository static analysis and linting scripts from the dependency manifest
# Example discovery / execution:
if [ -f package.json ]; then
  npm run lint || npx eslint .
elif [ -f pom.xml ]; then
  mvn checkstyle:check
elif [ -f build.gradle ]; then
  ./gradlew check
fi

# Discover and execute the package test suite from the repository manifest
if [ -f package.json ]; then
  npm test
elif [ -f pom.xml ]; then
  mvn test
elif [ -f build.gradle ]; then
  ./gradlew test
fi
```

**Accept when:**
- Static analysis validates that all logging calls use designated console methods with standardized component prefix strings.
- Unit and integration tests verify that error fallback paths execute and emit diagnostic error messages without crashing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>