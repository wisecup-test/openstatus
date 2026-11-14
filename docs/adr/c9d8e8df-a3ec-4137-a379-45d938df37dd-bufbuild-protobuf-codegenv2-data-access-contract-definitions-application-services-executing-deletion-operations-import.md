---
version: 2
name: c9d8e8df-a3ec-4137-a379-45d938df37dd-bufbuild-protobuf-codegenv2-data-access-contract-definitions-application-services-executing-deletion-operations-import
description: "@bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Application Services Executing Deletion Operations Import"
metadata:
  actual:
    adr_id: c9d8e8df-a3ec-4137-a379-45d938df37dd
    status: accepted
    candidate_group_id: 2198f7d3-4116-4221-bdc7-c0a86d693c7e
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/openstatus/monitor/v1/**
---

# @bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Application Services Executing Deletion Operations Import

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/proto`

Applies to:
- `gen/ts/openstatus/monitor/v1/**`

## Context

- Contract-driven architectures require consistent schema serialization for data mutation and deletion operations.
- The codebase contains generated serialization definitions utilizing @bufbuild/protobuf/codegenv2 to model deletion access actions.
- Evidence indicates a single generated schema defining deletion access types, presenting narrow evidence for project-wide data access standardization.

## Problem Statement

Manual definitions of deletion payloads across disparate services introduce schema divergence and runtime typing mismatches when executing data access operations.

## Decision

1. SHOULD: Application services executing deletion operations SHOULD import generated contract structures directly from their generated module boundaries rather than redefining access models.

## Policy Block

- SHOULD Application services executing deletion operations SHOULD import generated contract structures directly from their generated module boundaries rather than redefining access models.

In scope:
- Codebases generating TypeScript data contracts and deletion schema representations using @bufbuild/protobuf/codegenv2
- Components modeling data access deletion requests across boundary definitions

Out of scope:
- Internal application state management independent of protocol message schemas
- Direct database access layers that do not interact with serialized protocol contracts

## Rationale

- Utilizing @bufbuild/protobuf/codegenv2 ensures consistent, strongly-typed contract structures for data operations across client and server boundaries.
- Generated schema representations eliminate handwritten contract duplication for deletion payloads.
- Static detection identified deletion access structures within a single generated schema file, justifying low architectural confidence pending broader project adoption.

## Consequences

Positive:
- Eliminates type drift between schema definitions and operational deletion payloads.
- Enforces deterministic serialization contracts across service boundaries.
- Provides compile-time verification for deletion access models.

Negative:
- Introduces reliance on generated schema artifacts within application code.
- Increases dependency coupling to the specific code generation runtime.
- May obscure whether deletion semantics represent domain models or protocol-level transport payloads.

## Alternatives

- Manual TypeScript interface declarations for deletion requests (rejected)
  Rejected because: Manual interfaces diverge easily from upstream schema definitions and lack automated serialization guarantees.
  When valid: Prototypes or standalone micro-utilities without cross-service contract definitions.
- Generic untyped request payloads for delete operations (rejected)
  Rejected because: Omits compile-time validation for essential deletion parameters and identifiers.
  When valid: Dynamic proxy layers that pass payloads through without inspection.

## Risks

- Generated contract updates could introduce breaking changes to existing deletion consumers.
  Mitigation: Verify schema backwards compatibility during continuous integration prior to publishing updated types.
  Owner: Engineering Team
- Over-reliance on generated transport schemas for internal domain access patterns.
  Mitigation: Separate transport-level deletion contracts from internal storage domain entities.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Verify that all data access modules consume deletion types exported from the designated generated package rather than ad-hoc local types.
- Ensure schema generation workflows are integrated into build pipelines so generated types remain synchronized with canonical definitions.

## Continuation Context


Verify commands:
- echo "Discover and execute project test suite for schema compliance"
- echo "Discover and execute project type validation suite"
- echo "Discover and execute project contract linting suite"

Accept when:
- All deletion operations consuming protocol contracts compile against @bufbuild/protobuf/codegenv2 generated definitions.
- The project dependency lock artifact is inspected and verified to contain the resolved runtime dependency.
- Type validation checks pass across all modules handling deletion access patterns.

## Enforcement

- Verified by: Automated continuous integration type validation pipelines
- Verified by: Architecture and pull request code reviews
- Violation handling: Pull requests introducing manual overrides or ungenerated contract structures for deletion operations will be blocked.
- Violation handling: Code review will require refactoring hand-crafted payloads to use generated definitions.
- Exception process: Exceptions require documented architecture review and justification when interfacing with non-standard external services.