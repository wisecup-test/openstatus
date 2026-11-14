---
version: 2
name: d7f9ad62-e67c-4c40-97f8-8a017b541c63-adoption-of-openstatus-db-src-schema-for-proxy-resolution-and-access-control-proxy-handlers-evaluating-access-restrictions-domain
description: "Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Proxy Handlers Evaluating Access Restrictions Domain"
metadata:
  actual:
    adr_id: d7f9ad62-e67c-4c40-97f8-8a017b541c63
    status: accepted
    candidate_group_id: f8b4ae84-5a62-426c-ba59-a50291856ca2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Proxy Handlers Evaluating Access Restrictions Domain

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/lib/proxy/**`

## Context

- The status page proxy routing pipeline validates access policies and computes domain rewrite targets based on database-backed tenant configurations.
- Sharing core data models from @openstatus/db/src/schema into proxy resolution modules ensures type synchronization across data persistence and edge request processing.
- Decoupling route evaluation helpers from database access drivers while binding them to @openstatus/db/src/schema types maintains clear architectural boundaries.

## Problem Statement

Proxy routing and access control mechanisms require accurate representations of tenant data models to enforce access restrictions and execute path rewrites. Without a unified schema dependency, proxy resolvers risk data model drift, redundant interface maintenance, and runtime access policy mismatches.

## Decision

1. SHOULD: Proxy handlers evaluating access restrictions or domain rewrites SHOULD isolate schema field evaluations within modular access predicates and path builders.

## Policy Block

- SHOULD Proxy handlers evaluating access restrictions or domain rewrites SHOULD isolate schema field evaluations within modular access predicates and path builders.

In scope:
- Status page proxy resolution handlers, access control predicate evaluators, and request rewrite logic.

Out of scope:
- Client-side rendering components and static asset delivery pipelines that do not interact with proxy access rules or tenant routing configurations.

## Rationale

- Importing @openstatus/db/src/schema establishes a compile-time contract between database persistence models and proxy access evaluation logic.
- Centralizing domain schema definitions in a dedicated package reduces maintenance overhead and prevents type drift between database records and routing decisions.
- Evidence across proxy action resolvers demonstrates consistent reuse of schema structures alongside specialized predicate functions and path builders.

## Consequences

Positive:
- Ensures complete type safety and structural alignment between database records and proxy request evaluation.
- Eliminates redundant data model declarations across the proxy routing subsystem.
- Enables centralized refactoring and static type validation when domain entities change.

Negative:
- Couples proxy routing modules directly to the shared database schema package dependency tree.
- Schema modifications may require cascading updates and rebuilds across proxy resolution modules.

## Alternatives

- Duplicate Data Transfer Object Definitions in Proxy Modules (rejected)
  Rejected because: Introduces high risk of schema drift and synchronization defects between database entities and proxy access rules.
  When valid: When proxy handlers must execute in isolated runtimes with strict zero-dependency packaging constraints.
- Runtime Dynamic Schema Validation Without Compile-Time Types (rejected)
  Rejected because: Lacks compile-time safety and introduces runtime overhead during request resolution.
  When valid: When proxy rules are dynamically ingested from untrusted external inputs without compile-time schema definitions.

## Risks

- Database schema changes break proxy request resolution compilation.
  Mitigation: Require automated type-check and integration test suites on all schema modifications across dependent workspaces.
  Owner: engineering team
- Overly broad schema imports bloat proxy bundle size or runtime memory footprint.
  Mitigation: Use tree-shakable exports and selective type-only imports where runtime database drivers are not required.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import schema models as types whenever runtime database query builder objects are not required in proxy resolution.
- Encapsulate schema evaluation logic inside dedicated predicate functions to maintain single-responsibility separation in routing handlers.

## Continuation Context


Verify commands:
- Discover and run the repository type-checking script to verify cross-package schema compatibility across proxy resolution handlers.
- Discover and execute the test runner for proxy routing resolution to validate access predicate behavior against schema models.
- Discover and execute the repository dependency boundary linter to confirm adherence to package import boundaries.

Accept when:
- Type checking passes without errors across all proxy resolution handlers importing @openstatus/db/src/schema.
- All proxy access control and rewrite unit tests pass successfully.
- No duplicate local interface definitions exist for entities provided by @openstatus/db/src/schema.

## Enforcement

- Verified by: Automated continuous integration type checking and static analysis verification.
- Verified by: Peer code review for proxy routing modifications and schema updates.
- Violation handling: Pull request blocks on type-checking failures or duplicate schema declarations.
- Violation handling: Required remediation before merging any changes violating package boundary rules.
- Exception process: Formal architectural review request documenting technical constraints preventing shared schema usage.