---
version: 2
name: c420b2a2-0a68-40d0-a44a-9f82e17793ff-next-intl-server-adoption-for-server-side-internationalization-server-actions-operating-within-localized-route
description: "next-intl/server Adoption for Server-Side Internationalization: Server Actions Operating Within Localized Route"
metadata:
  actual:
    adr_id: c420b2a2-0a68-40d0-a44a-9f82e17793ff
    status: accepted
    candidate_group_id: c77b2748-b572-41ef-9ec0-64178f3643db
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/i18n/**
---

# next-intl/server Adoption for Server-Side Internationalization: Server Actions Operating Within Localized Route

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/i18n/**`

## Context

- User-facing status page interfaces require localized routing segments, internationalized layout rendering, and localized server-side action responses.
- Handling localization exclusively on the client introduces hydration mismatch risks, translation loading flashes, and localized error reporting gaps during server actions.
- Static analysis of the application route tree confirms recurring integration of next-intl/server across request configuration, root layout providers, and authentication server actions.

## Problem Statement

Serving multi-locale web applications without a unified server-side internationalization layer causes fragmented locale extraction, duplicated translation catalog loaders, client-side rendering flashes, and inconsistent error messaging within server actions.

## Decision

1. MUST: Server actions operating within localized route scopes MUST resolve locale-aware messages and routing redirects exclusively through next-intl/server.

## Policy Block

- MUST Server actions operating within localized route scopes MUST resolve locale-aware messages and routing redirects exclusively through next-intl/server.

In scope:
- Server-rendered route segments and layouts requiring localized content presentation.
- Request configuration handlers responsible for locale detection and message catalog resolution.
- Server actions executing within localized route scopes requiring localized responses or error handling.

Out of scope:
- Non-localized internal utility scripts operating outside user-facing route segments.
- Static assets and background job workers independent of request-bound locale context.

## Rationale

- Adopting next-intl/server establishes a single authoritative mechanism for resolving message catalogs and locale context during request evaluation and server action execution.
- Server-side message resolution ensures pages arrive with fully localized content, minimizing client bundle overhead and preventing hydration mismatches.
- Integrating request configuration with localized route layouts ensures cohesive locale propagation throughout the entire server component subtree.

## Consequences

Positive:
- Standardizes server-side localization and message resolution across layouts, request boundaries, and server actions.
- Eliminates client-side translation hydration delays and flashes of untranslated content on initial page delivery.
- Provides structured request-time locale resolution integrated natively with application routing segments.

Negative:
- Directly couples request lifecycle configuration, layout composition, and server actions to the next-intl runtime APIs.
- Requires explicit server-side context propagation for translation catalogs across nested server component hierarchies.
- Requires disciplined version alignment between server and client internationalization packages across major library upgrades.

## Alternatives

- Client-side only internationalization (rejected)
  Rejected because: Causes layout shift and flashes of untranslated content during initial rendering while preventing localized message resolution in server actions and server components.
  When valid: Static single-page applications lacking server-side rendering or localized server actions.
- Custom homegrown request-header locale extraction and dictionary loading (rejected)
  Rejected because: Requires maintaining custom message loaders, pluralization logic, and request context plumbing across layouts and server boundaries, increasing maintenance overhead.
  When valid: Environments with strict zero-external-dependency constraints or non-standard routing architectures.

## Risks

- Inconsistent message catalog loading across distinct server execution boundaries and server actions.
  Mitigation: Enforce centralized request configuration and validate server action translation resolution via automated integration tests.
  Owner: engineering team
- Upstream breaking changes across major library releases disrupting server request handling.
  Mitigation: Enforce lock artifact verification and automated test execution across localized route fixtures before applying dependency updates.
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
- Centralize internationalization request handling by defining a dedicated request configuration that derives locale parameters and loads corresponding translation messages before layout composition.
- Ensure server action routines obtain localized error formatting and redirection primitives from server-side internationalization exports rather than client-oriented hooks.

## Continuation Context


Verify commands:
- Discover and run the project test runner script defined in the repository configuration to execute all test suites covering localized layouts and server actions.
- Discover and execute the repository type-checking script to confirm full type compliance for server-side internationalization imports and request handlers.
- Discover and run the repository linter script to ensure no unauthorized internationalization imports exist across server modules.

Accept when:
- All localized route layout test fixtures and server action verification suites pass without error.
- Static analysis and type checking validate that all server-side localization helpers resolve strictly against next-intl/server.
- Continuous integration pipelines report clean verification runs with no conflicting internationalization dependencies detected.

## Enforcement

- Verified by: Automated continuous integration pipelines executing type checking and localized test suites.
- Verified by: Architecture review and code review verification for pull requests touching localized route trees or server actions.
- Violation handling: Pull requests introducing unapproved localization libraries or bypassing server request configuration fail automated verification and are blocked from merging.
- Exception process: Exceptions require architectural review approval and documented rationale filed in the project issue tracker prior to implementation.