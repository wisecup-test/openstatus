---
version: 2
name: 6d233f00-d6e7-496e-95a7-e9262e384b15-adoption-of-openstatus-services-module-for-centralized-domain-logic-route-handlers-webhook-endpoints-context-middleware
description: "Adoption of @openstatus/services Module for Centralized Domain Logic: Route Handlers Webhook Endpoints Context Middleware"
metadata:
  actual:
    adr_id: 6d233f00-d6e7-496e-95a7-e9262e384b15
    status: accepted
    candidate_group_id: d7fa39ec-eb90-4ffa-b65a-12044e6ce4ef
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/services Module for Centralized Domain Logic: Route Handlers Webhook Endpoints Context Middleware

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/api/**`
- `src/lib/**`

## Context

- Application endpoints, webhook listeners, edge context resolvers, and autonomous agent tools require access to shared business rules and domain operations.
- Directly implementing domain logic and data transformations inside request handlers and tool adapters introduces duplication, divergent behavior, and testing complexity.
- The codebase establishes the internal package @openstatus/services as the shared domain logic layer, providing root and domain-specific subpath entry points across application features.

## Problem Statement

Decentralized business logic scattered across transport layers, context builders, and agent integrations leads to inconsistent execution of core workflows, fragmented validation, and maintenance overhead. The architecture requires a unified, reusable services module that encapsulates domain workflows while presenting clean domain-scoped interfaces.

## Decision

1. MUST_NOT: Route handlers, webhook endpoints, and context middleware MUST NOT directly execute unencapsulated business logic or bypass domain boundaries defined by @openstatus/services.

## Policy Block

- MUST_NOT Route handlers, webhook endpoints, and context middleware MUST NOT directly execute unencapsulated business logic or bypass domain boundaries defined by @openstatus/services.

In scope:
- Application route handlers, edge context resolvers, webhook consumers, and agent tooling consuming domain business capabilities.
- Shared domain logic, external service integrations, and business workflow orchestration.

Out of scope:
- Pure presentation components and styling layers that do not interact with application state or backend domains.
- Low-level data store drivers and connection utilities that are encapsulated behind database access layers.

Exceptions:
- EXC-20-001: A transport layer requires a trivial static response or health probe that does not read or mutate domain state.

## Rationale

- Centralizing domain operations inside @openstatus/services standardizes business logic across disparate consumer modules including webhooks, API routes, and agent integrations.
- Subpath entry points within @openstatus/services provide modular domain separation, allowing discrete capabilities to be imported without loading the entire service graph.
- Static analysis across route handlers, agent tools, and context layers confirms that domain workflows consistently route through @openstatus/services rather than ad-hoc inline routines.

## Consequences

Positive:
- Promotes code reuse and single-point maintenance for core business processes across API endpoints and agent workflows.
- Maintains thin transport handlers and clean separation of concerns between protocol handling and domain computation.
- Facilitates standardized validation and error handling across service boundaries.

Negative:
- Creates a centralized dependency where breaking changes in service interfaces impact multiple downstream consumers.
- Requires maintaining clean subpath exports and modular boundaries within the service package to prevent circular dependencies.

## Alternatives

- Inline domain business logic directly within individual route handlers and agent adapters (rejected)
  Rejected because: Leads to duplicated domain rules, fragmented validation logic, and high risk of inconsistent behavior between HTTP APIs and agent tooling
  When valid: Prototyping disposable single-use scripts that will not be maintained as part of the core product
- Monolithic single-entry service layer exporting all domain methods through one root namespace (rejected)
  Rejected because: Obscures domain boundaries and increases bundle coupling compared to domain-scoped subpath entry points
  When valid: Small codebases with fewer than three distinct domain operations

## Risks

- Service layer interface churn causing widespread compile and runtime breakage across consumers
  Mitigation: Enforce contract-first interface versioning and run automated integration suites across all dependent packages before publishing changes
  Owner: Core Engineering Team
- Accidental leaking of transport concerns such as HTTP request objects into domain service implementations
  Mitigation: Strictly enforce decoupled parameter contracts requiring plain data transfers validated by schemas at service boundaries
  Owner: Backend Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Structure domain services with dedicated subpath exports to maintain clear boundary segregation between independent capabilities.
- Ensure all consumer calls to service functions pass structured parameters that have been validated at the integration boundary.

## Continuation Context


Verify commands:
- Discover the workspace dependency manifest and run the repository type verification script to ensure all consumers conform to service interfaces.
- Discover and run the project test execution script targeting domain service modules and consuming endpoints.
- Discover and execute the lint and module boundary validation suite to detect unauthorized direct dependencies or bypassed service layers.

Accept when:
- Static type verification succeeds across all consumers of @openstatus/services without interface mismatches.
- All unit and integration test suites testing domain services and consumer endpoints pass with zero failures.
- Architecture boundary checks verify that domain logic in route handlers and agent tools is invoked exclusively through @openstatus/services.

## Enforcement

- Verified by: Automated continuous integration pipelines validating static types and running unit and integration test suites on pull requests.
- Verified by: Automated architecture and dependency boundary linters verifying imports conform to approved module paths.
- Verified by: Peer code reviews confirming business logic is not duplicated outside the service layer.
- Violation handling: Continuous integration builds fail upon type mismatches, boundary violations, or failing service tests, blocking pull request merges.
- Violation handling: Unapproved inline domain logic identified during review must be refactored into @openstatus/services before merge.
- Exception process: Submit an architectural review request outlining why a domain workflow cannot be placed within @openstatus/services, accompanied by sign-off from the technical lead.