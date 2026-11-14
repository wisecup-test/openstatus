---
version: 2
name: 726a4d4e-98fd-4486-b983-7e6fb6af2364-next-js-runtime-environment-variable-configuration-environment-variables-intended-consumption-within-browser
description: "Next.js Runtime Environment Variable Configuration: Environment Variables Intended Consumption Within Browser"
metadata:
  actual:
    adr_id: 726a4d4e-98fd-4486-b983-7e6fb6af2364
    status: accepted
    candidate_group_id: 01a7d574-6a6a-490d-a13f-d7c47d691d1a
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - src/**
---

# Next.js Runtime Environment Variable Configuration: Environment Variables Intended Consumption Within Browser

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `src/**`

## Context

- The application requires diverse configuration sources across server routes, background cron tasks, shared client-server communication modules, and frontend instrumentation
- Static analysis identifies twenty-seven files directly accessing process environment variables to parameterize endpoints, authentication tokens, runtime targets, and operational modes
- A standardized framework configuration convention is necessary to prevent secret leakage across execution boundaries while supporting flexible deployment injection

## Problem Statement

Direct consumption of runtime environment variables across disparate application modules risks runtime failures from missing values and unintended credential exposure in client bundles if not governed by clear architectural rules.

## Decision

1. MUST: Environment variables intended for consumption within browser contexts MUST adhere to the framework designated public prefix convention.

## Policy Block

- MUST Environment variables intended for consumption within browser contexts MUST adhere to the framework designated public prefix convention.

In scope:
- Server-side route handlers, background cron jobs, and server-rendered components within the web application
- Client instrumentation hooks and shared protocol utilities requiring runtime environment parameters

Out of scope:
- Static assets and presentation styling definitions that do not participate in runtime code execution
- Pure algorithmic and utility functions that operate strictly on explicit invocation parameters without environment dependencies

## Rationale

- Direct consumption of process environment properties provides seamless compatibility with platform runtime injection and eliminates unnecessary indirection layers across route handlers and utility modules
- Enforcing prefix-based public isolation and strict server boundaries prevents confidential tokens from leaking into client-side browser bundles
- Twenty-seven detected source files consistently employ runtime process environment variables for service endpoints, secrets, and environment indicators

## Consequences

Positive:
- Eliminates intermediate configuration abstraction overhead by directly consuming runtime environment properties at point-of-use
- Aligns naturally with platform runtime injection across preview, staging, and production deployment environments
- Maintains clean runtime separation between server-only secrets and browser-accessible public telemetry configuration

Negative:
- Missing or misconfigured environment variables can produce late runtime exceptions at point-of-use rather than failing fast during boot initialization
- Developers must manually ensure that private credentials are not inadvertently referenced within client-rendered component trees

## Alternatives

- Centralized boot-time schema validation layer wrapping all environment variables (rejected)
  Rejected because: Direct point-of-use consumption of process environment properties is already established across existing route handlers, background jobs, and shared modules without intermediate schema wrappers
  When valid: When strict boot-time schema validation across all environment variables is mandated prior to application initialization
- Hardcoded deployment configuration constants within application source files (rejected)
  Rejected because: Hardcoded configuration tightly couples deployment artifacts to specific runtime infrastructure and risks exposing sensitive credentials
  When valid: When configuration values are universally immutable across all deployment environments and carry no confidentiality requirements

## Risks

- A missing environment variable causes an unhandled runtime failure during route execution rather than at application startup
  Mitigation: Implement presence checks and descriptive error logging at the boundary of external client initializers
  Owner: engineering team
- Server-only secrets or private webhook endpoints are accidentally included in client-facing bundles
  Mitigation: Enforce static bundle analysis and code review checks preventing server environment variables from being referenced in client components
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
- Ensure that all environment variable reads include appropriate fallback values or explicit validation when invoked inside critical execution paths
- Maintain strict boundary isolation to guarantee that private authorization keys and webhook targets remain confined to server execution contexts

## Continuation Context


Verify commands:
- echo "Discover project verification scripts from repository manifest and execute environment check commands"
- echo "Inspect codebase for direct process environment variable references across module boundaries"

Accept when:
- All server route handlers, background tasks, and shared client-server modules resolve required runtime configuration properties without unhandled runtime exceptions
- Client-side bundles remain completely free of server-only secrets and private credential references

## Enforcement

- Verified by: Continuous integration build pipelines validating compilation and bundle boundaries between server and client modules
- Verified by: Peer code review auditing point-of-use environment variable references and credential protection
- Violation handling: Pull requests introducing direct access to unapproved environment variables or referencing server secrets in client contexts are blocked from merging until resolved
- Exception process: Exceptions for non-standard configuration sources or transitional environment bindings require architectural review approval and documented rationale in issue tracking