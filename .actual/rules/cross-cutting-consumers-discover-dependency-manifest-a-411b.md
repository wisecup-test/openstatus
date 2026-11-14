---
version: 2
name: cross-cutting-consumers-discover-dependency-manifest-a-411b
description: "Fetch API Client Boundary Integration: Consumers Discover Dependency Manifest Authoritative Resolution"
metadata:
  actual:
    adr_id: 411bf326-ae3e-4a97-a89b-7c3dd9db18d9
    status: accepted
    candidate_group_id: 57dd9a6d-921b-457c-a0a1-87439126474c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Fetch API Client Boundary Integration: Consumers Discover Dependency Manifest Authoritative Resolution

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FABI-001** MUST: Consumers MUST discover the dependency manifest and authoritative resolution lock artifact to verify runtime compatibility and exact versions of all supporting libraries before implementation.
- **R-FABI-002** MUST: Execute in order before writing code that uses a versioned library: (1) Find the dependency manifest in the repo declaring ranges, (2) Identify the build tool from the manifest, (3) Inspect the repository lock or resolution artifact to determine the exact resolved version as authoritative, (4) Look up official documentation/changelog/public API reference for that exact version without relying on training-data recall, (5) Confirm every API, class, or function called exists in that exact version's documentation, (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-FABI-003** MUST: Configure timeout and abort controllers uniformly using standard abort signals across client-side interaction triggers and background probing routines.
- **R-FABI-004** MUST: Pair fetch operations with schema-based validation layers when ingesting third-party payloads or parsing route responses.

### Verify

```bash
# Discover and run the project static analysis and linting command from the root configuration
# Discover and execute the project test runner across all integration test suites
# Discover and execute the project type checking script from the workspace manifest
```

**Accept when:**
- Static analysis validates that all boundary network requests conform to the standard client rules without unapproved HTTP dependencies.
- All integration test suites covering internal routes and external boundary clients pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>