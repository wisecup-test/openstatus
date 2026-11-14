---
version: 2
name: cross-cutting-domain-operations-that-coordinate-cross-bb77
description: "Adoption of @openstatus/services as the Shared Domain Service Module: Domain Operations That Coordinate Cross Boundary"
metadata:
  actual:
    adr_id: bb775116-5bc9-4c3c-863a-61bdbc1b0738
    status: accepted
    candidate_group_id: 59a6b1ea-bddf-485b-a89f-dea1337ef130
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @openstatus/services as the Shared Domain Service Module: Domain Operations That Coordinate Cross Boundary

Scope: applies to `apps/server/src/routes/slack/**`.

These rules are ALWAYS ACTIVE for server route handlers, webhook receivers, and third-party integration adapters executing domain actions.

### Rules

- **R-DOM-001** SHOULD: Domain operations that coordinate cross-boundary workflows across external integrations and local state stores SHOULD be encapsulated as cohesive service methods within @openstatus/services.
- **R-DOM-002** MANDATORY: Locate the workspace package declaration for @openstatus/services within the monorepo structure to inspect its public interface exports and type definitions before implementing route-level logic.
- **R-DOM-003** MANDATORY: When adding endpoints that require interaction confirmation or asynchronous domain action execution, ensure service calls decouple request parsing from workflow execution.
- **R-DOM-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation/changelog/public API reference for that exact version. (5) Confirm every API, class, or function you will call exists in that exact version's documentation before using it. (6) For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover the root project manifest and execute the designated workspace test suite covering route integration and service adapter modules.
# Discover the repository linter and type-checking scripts from the workspace configuration to validate import boundary compliance across route handlers.
```

**Accept when:**
- All server route handlers and protocol adapters route domain operations through @openstatus/services rather than executing domain logic inline.
- Project verification scripts and static analysis suites complete with zero boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and peer code review.
</enforcement>