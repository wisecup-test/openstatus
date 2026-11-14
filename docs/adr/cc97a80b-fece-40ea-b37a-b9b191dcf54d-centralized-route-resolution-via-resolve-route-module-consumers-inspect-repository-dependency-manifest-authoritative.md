---
version: 2
name: cc97a80b-fece-40ea-b37a-b9b191dcf54d-centralized-route-resolution-via-resolve-route-module-consumers-inspect-repository-dependency-manifest-authoritative
description: "Centralized Route Resolution via resolve-route Module: Consumers Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: cc97a80b-fece-40ea-b37a-b9b191dcf54d
    status: accepted
    candidate_group_id: c1192f46-e2dc-4500-93bd-53907a2f23e6
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Centralized Route Resolution via resolve-route Module: Consumers Inspect Repository Dependency Manifest Authoritative

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/lib/proxy/**`

## Context

- The proxy subsystem requires consistent URL rewriting, locale override application, and external target path construction across multiple request handling utilities.
- Dispersed and ad-hoc route parsing logic across proxy routines risks path drift, inconsistent prefix handling, and broken routing behavior.
- The codebase establishes centralized routing resolution by importing the internal resolve-route module across proxy transformation modules.

## Problem Statement

Proxy request transformation utilities must accurately resolve routes, construct external target paths, and apply locale overrides without duplicating path resolution logic or introducing inconsistent routing behavior across independent transformation functions.

## Decision

1. MUST: Consumers MUST inspect the repository dependency manifest and authoritative lock artifact to verify workspace resolution integrity before modifying routing dependencies.

## Policy Block

- MUST Consumers MUST inspect the repository dependency manifest and authoritative lock artifact to verify workspace resolution integrity before modifying routing dependencies.

In scope:
- Proxy request handling, URL construction, locale override resolution, and path prefix normalization routines.

Out of scope:
- Direct database schema migrations and independent external network clients that do not process incoming proxy routes.

## Rationale

- Centralizing route resolution within the resolve-route module ensures consistent path handling across all proxy transformations.
- Static dependency analysis demonstrates that proxy utility functions consistently delegate route operations to the resolve-route module.
- Reusing a single routing module eliminates redundant string manipulation and reduces regression risk during routing specification changes.

## Consequences

Positive:
- Single point of route resolution logic prevents divergent routing behaviors across proxy utilities.
- Simplifies maintenance and future path transformation changes by encapsulating path parsing in one module.
- Standardizes routing interfaces and type contracts across proxy helper functions.

Negative:
- Introduces an architectural dependency where proxy utilities are tightly coupled to the resolve-route module interface.
- Modifications to the resolve-route contract require coordinated verification across all consuming proxy functions.

## Alternatives

- Decentralized inline path parsing and regex matching within each proxy utility (rejected)
  Rejected because: Increases maintenance burden and creates high risk of route handling inconsistency across proxy functions
  When valid: Valid only in trivial single-file prototypes where shared abstractions add unnecessary overhead
- External third-party URL rewrite middleware (rejected)
  Rejected because: Lacks domain-specific awareness of application locale overrides and custom route definitions
  When valid: Valid for generic reverse proxies with static routing rules

## Risks

- Breaking changes in resolve-route module could cause runtime route resolution failures across all proxy utilities.
  Mitigation: Maintain rigorous automated test coverage for all exported route resolution contracts before deploying changes.
  Owner: Engineering Team
- Performance overhead from redundant route parsing across multiple intermediate proxy functions.
  Mitigation: Ensure route resolution results are cached or reused within the request context.
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
- Ensure all new proxy endpoints, request transformers, and path rewrites delegate path parsing and URL normalization directly to the shared resolve-route module.
- Validate that schema attributes and route definitions in the proxy layer stay synchronized with data model schemas and route specifications.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and linting scripts from the dependency manifest and execute them to verify that all proxy path manipulation utilities import and utilize resolve-route.
- Discover and run the project type checking and test suite scripts to validate route resolution contract compliance.

Accept when:
- All proxy path transformation functions import and route path parsing through the resolve-route module.
- Repository test suites and type verification scripts pass without route resolution errors.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test suites and static analysis.
- Verified by: Mandatory peer code review for any changes affecting proxy path manipulation or the resolve-route module.
- Violation handling: Pull requests introducing ad-hoc path parsing in proxy handlers will fail review and must be refactored to use resolve-route.
- Violation handling: Static analysis violations in continuous integration will block deployment.
- Exception process: Exceptions require documented architectural justification and written sign-off from the engineering team lead before merging.