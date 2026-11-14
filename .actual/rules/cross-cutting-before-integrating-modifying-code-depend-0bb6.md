---
version: 2
name: cross-cutting-before-integrating-modifying-code-depend-0bb6
description: "Structured Logging with log/slog: Before Integrating Modifying Code Dependent Structured"
metadata:
  actual:
    adr_id: 0bb6b789-9c61-4f17-b944-cee74b033fd4
    status: accepted
    candidate_group_id: 85876990-3f0b-4766-bd31-5ea7eb5f9942
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Structured Logging with log/slog: Before Integrating Modifying Code Dependent Structured

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for server applications, initialization routines, and background worker modules requiring structured operational visibility.

### Rules

- **R-LOG-001** MUST: Before integrating or modifying code dependent on the structured logging package, the consumer MUST inspect the repository dependency manifest and lock artifact to verify runtime version compatibility.
- **R-LOG-002** DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-LOG-003** LOCK-VERSION GROUNDING (MANDATORY): Before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-LOG-004** MUST: Every structured log call MUST maintain balanced key-value pairs following the constant message string.
- **R-LOG-005** MUST: Log messages MUST remain constant and concise to serve as stable event identifiers across log aggregators.

### Verify

```bash
test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Repository root verified; discover and execute project test scripts"
test -n "$(git rev-parse --show-toplevel 2>/dev/null)" && echo "Discover and execute project linter scripts for structured logging verification"
```

**Accept when:**
- All project static analysis and linting scripts pass with zero errors regarding structured logging calls.
- All automated test suites execute and pass successfully across all server packages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>