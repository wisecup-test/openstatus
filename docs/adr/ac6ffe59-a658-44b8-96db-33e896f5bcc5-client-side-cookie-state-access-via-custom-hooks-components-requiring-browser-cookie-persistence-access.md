---
version: 2
name: ac6ffe59-a658-44b8-96db-33e896f5bcc5-client-side-cookie-state-access-via-custom-hooks-components-requiring-browser-cookie-persistence-access
description: "Client-Side Cookie State Access via Custom Hooks: Components Requiring Browser Cookie Persistence Access"
metadata:
  actual:
    adr_id: ac6ffe59-a658-44b8-96db-33e896f5bcc5
    status: accepted
    candidate_group_id: 6c7ae462-c185-46d7-993b-d6604a9c537b
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/hooks/**
---

# Client-Side Cookie State Access via Custom Hooks: Components Requiring Browser Cookie Persistence Access

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/hooks/**`

## Context

- Client-side interface components require localized persistence across browser sessions using standard browser storage mechanisms.
- Accessing browser cookie state directly across UI components creates repetitive string manipulation logic and couples components to browser runtime APIs.
- The observed pattern encapsulates cookie inspection and parsing within a dedicated client-side state hook.

## Problem Statement

Directly reading and mutating browser cookies inside user interface components duplicates delimiter splitting logic and risks runtime exceptions during non-client execution contexts.

## Decision

1. MUST: Components requiring browser cookie persistence MUST access cookie values through designated client-side state hooks rather than invoking direct document cookie string operations inline.

## Policy Block

- MUST Components requiring browser cookie persistence MUST access cookie values through designated client-side state hooks rather than invoking direct document cookie string operations inline.

In scope:
- Client-side user interface components requiring browser cookie synchronization.

Out of scope:
- Server-rendered data operations or backend persistent storage systems.

## Rationale

- Encapsulating browser cookie string manipulation inside custom hooks prevents duplicate parsing logic across the user interface package.
- Utilizing client-side hook primitives isolates browser-specific APIs from server-side component execution.
- The evidence demonstrates single-file scope using standard language and browser built-ins rather than a multi-package architectural framework adoption.

## Consequences

Positive:
- Encapsulates cookie string parsing and state updates within a reusable client hook abstraction.
- Eliminates third-party client-side dependency overhead for basic cookie storage persistence.

Negative:
- Direct document cookie string manipulation is limited to browser runtime environments and cannot be evaluated during server passes without explicit client isolation.
- String-based delimiter matching requires manual sanitization and lacks schema validation for stored cookie values.

## Alternatives

- Direct inline document cookie parsing across individual UI components (rejected)
  Rejected because: Duplication of cookie string splitting and manual parsing logic increases maintenance overhead and regression risk.
  When valid: Valid only in standalone single-file prototypes where shared abstractions are unnecessary.
- Adoption of a dedicated third-party client state management dependency (rejected)
  Rejected because: Adds extraneous package dependencies for simple client-side cookie string synchronization.
  When valid: Valid when complex cross-tab synchronization or advanced storage persistence features are required.

## Risks

- Runtime exceptions occurring when evaluating document object references in server environments.
  Mitigation: Ensure client-only execution directives or environment runtime guards enclose any document object access.
  Owner: engineering team
- Inaccurate cookie value extraction caused by unusual or unescaped delimiter characters.
  Mitigation: Implement encoding and decoding utilities to avoid corruption from malformed delimiter strings.
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
- Ensure state hooks interacting with browser document objects are guarded against non-browser runtime contexts.
- Validate that cookie names and values are appropriately encoded and decoded when serialized.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and linting scripts from the repository manifest.
- Discover and run the project's unit and hook test suites via the primary repository test runner.

Accept when:
- Client-side cookie synchronization logic encapsulates document object parsing without leaking direct cookie string operations across consumer components.
- Test suites and static analysis pass when executing repository validation routines.

## Enforcement

- Verified by: Automated static code analysis and lint rules in continuous integration.
- Verified by: Peer review during code integration checks.
- Violation handling: Flag pull requests that execute unencapsulated document cookie manipulation outside designated state hooks.
- Violation handling: Block integration until cookie interactions adhere to the encapsulated hook pattern.
- Exception process: Submit an architectural change request detailing why an alternative client storage mechanism or dedicated library is required.
- Exception process: Obtain sign-off from the technical leads prior to introducing external storage management dependencies.