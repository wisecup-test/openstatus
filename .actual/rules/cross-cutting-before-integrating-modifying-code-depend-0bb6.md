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

These rules are ALWAYS ACTIVE for server initialization routines, background asynchronous jobs, worker dispatch failure reporting, HTTP server request handlers, and RPC server request handler diagnostic logging.

### Rules

- **R-SLOG-001** MUST: Before integrating or modifying code dependent on the structured logging package, the consumer MUST inspect the repository dependency manifest and lock artifact to verify runtime version compatibility.
- **R-SLOG-002** DISCOVERY POLICY (MANDATORY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-SLOG-003** LOCK-VERSION GROUNDING (MANDATORY): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect lock or resolution artifact to determine exact resolved version, look up official documentation/changelog/public API reference for that exact version without relying on training-data recall, confirm every API/class/function exists in that version's documentation, and re-run per dependency at point of use for version-sensitive behavior.
- **R-SLOG-004** MUST: Ensure that every structured log call maintains balanced key-value pairs following the constant message string.
- **R-SLOG-005** MUST: Keep log messages constant and concise to serve as stable event identifiers across log aggregators.

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