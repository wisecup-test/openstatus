---
version: 2
name: 6fcb8e73-4c65-4fdf-8176-ba50b4676cb1-adoption-of-openstatus-assertions-for-monitor-assertion-modeling-and-evaluation-service-modules-not-declare-duplicate-conflicting
description: "Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Service Modules Not Declare Duplicate Conflicting"
metadata:
  actual:
    adr_id: 6fcb8e73-4c65-4fdf-8176-ba50b4676cb1
    status: accepted
    candidate_group_id: 468c0ac4-a610-419a-85b8-fa4ec46ef6f8
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Service Modules Not Declare Duplicate Conflicting

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/monitor/**`

## Context

- Monitoring services require structured assertion criteria to evaluate endpoint responsiveness and verify expected service payloads.
- Embedding assertion logic directly into individual database access routines or endpoint schemas creates fragmented validation semantics.
- The service layer adopts the dedicated @openstatus/assertions library across monitor schemas and internal execution workflows to standardize assertion structures.

## Problem Statement

Defining monitor assertion structures independently within individual service routines leads to divergent validation criteria and duplicated evaluation logic across monitoring workflows. A centralized library module is required to ensure consistent validation schemas and uniform assertion structures across the service layer.

## Decision

1. MUST_NOT: Service modules MUST NOT declare duplicate or conflicting assertion schema types outside the dedicated assertion module.

## Policy Block

- MUST_NOT Service modules MUST NOT declare duplicate or conflicting assertion schema types outside the dedicated assertion module.

In scope:
- Monitor service schema definitions and monitor execution services that define or process health check assertions.

Out of scope:
- Domain services and infrastructure utilities that do not handle monitor health checks or HTTP response assertion processing.

## Rationale

- Static analysis identifies consistent imports of @openstatus/assertions alongside monitor database access and schema definitions.
- Centralizing assertion models in a dedicated module decouples validation primitives from transport schemas and persistence mechanisms.
- Structuring monitor configurations with uniform assertion arrays guarantees interoperability across monitor scheduling and evaluation pipelines.

## Consequences

Positive:
- Standardized assertion contracts eliminate schema discrepancies between monitor creation, storage, and execution.
- Centralized assertion definitions simplify testing and ongoing refinement of health check evaluation logic.
- Reusability of assertion schemas across multiple service routines avoids redundant validation declarations.

Negative:
- Modifications to assertion primitives require coordinated updates across schema definitions and service consumers.
- Couples monitor schema definitions and service routines to an internal module dependency across the service boundary.

## Alternatives

- Inline assertion schema definitions within each individual monitor service module (rejected)
  Rejected because: Duplicating assertion schemas across service modules introduces validation drift and maintenance overhead.
  When valid: Valid only in throwaway prototypes where assertions are not shared across components.
- Generic unstructured key-value assertion representations without domain schema enforcement (rejected)
  Rejected because: Untyped assertions fail to enforce required assertion parameters during input validation and evaluation.
  When valid: Valid when assertion criteria are opaque to the service layer and evaluated entirely by external third-party engines.

## Risks

- Breaking changes in the assertion module contract could invalidate stored monitor configuration schemas.
  Mitigation: Enforce strict backward compatibility and automated schema regression tests across service boundaries.
  Owner: Engineering Team
- Version skew between service consumers and the shared module could cause runtime evaluation failures.
  Mitigation: Follow the mandatory lock-version grounding policy and validate resolved dependencies prior to deployment.
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
- Import assertion models into monitor schema definitions to structure validation rules for monitor configuration arrays.
- Pass validated assertion structures directly from monitor schemas into internal service transaction handlers.

## Continuation Context


Verify commands:
- Discover the repository type verification script from the project manifest and execute it to confirm type compatibility with the assertions module.
- Discover and run the project test suite to verify monitor schema validation and assertion processing.

Accept when:
- All monitor service schemas validate assertion arrays using types defined in the shared assertions module.
- Repository type verification and automated test suites execute with zero failures across service modules.

## Enforcement

- Verified by: Automated type checking and static analysis during continuous integration pipelines.
- Verified by: Peer architectural review on pull requests modifying monitor schemas or service execution routines.
- Violation handling: Pull requests introducing ad-hoc assertion schemas or bypassing the shared assertions module are blocked from merging until refactored.
- Exception process: Exceptions require written justification explaining why standard assertion schemas cannot satisfy domain requirements.
- Exception process: Approval must be granted by the service architecture team prior to merging non-standard assertion implementations.