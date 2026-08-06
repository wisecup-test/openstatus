# Adopt Asynchronous API Key Generation and Verification Pattern: Key Verification Functions

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all authentication modules that generate, hash, or verify API keys.

## Context

- The system requires secure API key generation and storage for authenticating programmatic access to services
- Cryptographic operations for random token generation and password hashing are computationally expensive and block the event loop if executed synchronously
- The authentication module exposes public contracts for generating API keys, hashing tokens, verifying stored hashes, and determining credential refresh policies
- The codebase uses crypto for secure random byte generation and bcryptjs for adaptive hashing with configurable work factors
- All credential operations are exposed as asynchronous functions to prevent blocking I/O and maintain system responsiveness under concurrent authentication requests

## Problem Statement

Authentication systems must perform cryptographically secure operations for API key generation and verification without degrading system responsiveness or throughput when handling concurrent authentication requests. Synchronous cryptographic operations block the event loop and create latency spikes under load.

## Decision

1. MUST: All API key verification functions that compare tokens against stored hashes shall be implemented as asynchronous operations

## Policy Block

- MUST All API key verification functions that compare tokens against stored hashes shall be implemented as asynchronous operations

In scope:
- All authentication modules that generate API keys for programmatic access
- All credential storage operations that hash tokens before persistence
- All credential verification operations that compare provided tokens against stored hashes
- All public API contracts exposed for API key lifecycle management

Out of scope:
- User password authentication flows that use different credential types
- Session token generation and validation mechanisms
- OAuth or SAML federation flows that delegate authentication to external providers
- Synchronous cryptographic operations used for non-authentication purposes such as data encryption at rest

Exceptions:
- EXC-001: Cryptographic operations are performed in dedicated worker threads or processes that do not share the main event loop
- EXC-002: The authentication module operates in a synchronous runtime environment that does not support asynchronous operations

## Rationale

- The evidence shows all credential operations are exposed as asynchronous functions, indicating a deliberate architectural choice to prevent blocking the event loop during computationally expensive cryptographic operations
- Using crypto for random byte generation and bcryptjs for adaptive hashing demonstrates adherence to cryptographic best practices while requiring asynchronous execution to maintain system responsiveness
- The public API contracts for generateApiKey, hashApiKey, verifyApiKeyHash, and shouldUpdateLastUsed establish a consistent asynchronous interface for all credential lifecycle operations
- Asynchronous credential operations enable the system to handle concurrent authentication requests without latency degradation, supporting scalable API authentication patterns

## Consequences

Positive:
- System maintains responsiveness and throughput when processing concurrent authentication requests with computationally expensive cryptographic operations
- Event loop remains unblocked, allowing the runtime to interleave I/O operations and other asynchronous work during credential processing
- Consistent asynchronous API contracts simplify integration and testing by establishing predictable promise-based interfaces
- Architecture supports horizontal scaling by preventing single-threaded bottlenecks in authentication flows

Negative:
- Asynchronous operations introduce complexity in error handling, requiring proper promise rejection handling and async/await patterns throughout the call chain
- Debugging asynchronous credential operations is more complex than synchronous equivalents due to non-linear stack traces and timing dependencies
- Developers must understand promise semantics and event loop behavior to correctly implement and maintain credential operations
- Testing asynchronous credential flows requires additional tooling and patterns to handle timing, concurrency, and promise resolution states

## Alternatives

- Implement all credential operations synchronously using blocking cryptographic APIs (rejected)
  Rejected because: Synchronous cryptographic operations block the event loop, causing latency spikes and throughput degradation under concurrent authentication load. This approach is incompatible with scalable API authentication patterns.
  When valid: Only valid in single-threaded batch processing contexts where concurrency is not required and latency is not a concern
- Offload all cryptographic operations to dedicated worker threads or processes with synchronous APIs (rejected)
  Rejected because: While this approach prevents event loop blocking, it introduces significant architectural complexity with inter-process communication, thread pool management, and serialization overhead. The evidence shows direct asynchronous library usage rather than worker-based delegation.
  When valid: Valid when cryptographic operations are so computationally intensive that even asynchronous execution impacts event loop performance, or when integrating legacy synchronous cryptographic libraries
- Use hybrid synchronous generation with asynchronous hashing and verification (rejected)
  Rejected because: Inconsistent API contracts increase cognitive load and error potential. The evidence shows all credential operations follow a uniform asynchronous pattern, establishing a consistent interface.
  When valid: Valid only if random token generation is proven to be non-blocking and sufficiently fast that asynchronous execution provides no measurable benefit

## Risks

- Improper error handling in asynchronous credential operations may leak sensitive information through unhandled promise rejections or expose timing attack vectors
  Mitigation: Implement comprehensive error handling with consistent error types, ensure all promises have rejection handlers, and apply constant-time comparison for credential verification to prevent timing attacks
  Owner: Security team and authentication module maintainers
- Asynchronous operations may be incorrectly awaited or chained, leading to race conditions where credentials are used before hashing completes or verification results are checked before comparison finishes
  Mitigation: Enforce strict linting rules for promise handling, require comprehensive integration tests that verify correct async/await usage, and conduct code reviews focused on asynchronous control flow
  Owner: Engineering team
- Performance characteristics of asynchronous cryptographic operations may degrade under extreme concurrent load if the underlying library does not properly manage thread pool resources
  Mitigation: Establish performance benchmarks for concurrent authentication requests, monitor authentication latency in production, and implement rate limiting or queuing mechanisms to prevent resource exhaustion
  Owner: Platform architecture team and SRE team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the project's cryptographic library and verify it provides asynchronous APIs for random byte generation. Consult the library's documentation for the exact resolved version to determine the correct method signatures and return types.
- Discover the project's adaptive hashing library and verify it provides asynchronous APIs for both hash generation and comparison. Configure work factors according to the security policy's requirements for authentication latency and computational cost.
- Implement all credential operations using async/await syntax with comprehensive try-catch blocks. Ensure promise rejections are properly handled and logged without exposing sensitive credential data in error messages or stack traces.

## Continuation Context


Verify commands:
- Discover the project's test execution mechanism and run the authentication module's test suite to verify all credential operations return promises and complete without blocking
- Discover the project's static analysis configuration and execute linting rules that enforce proper async/await usage and promise handling in credential operations
- Discover the project's performance testing framework and execute load tests that verify authentication latency remains acceptable under concurrent request patterns

Accept when:
- All API key generation, hashing, and verification functions are implemented as asynchronous operations that return promises
- Static analysis confirms all credential operations use proper async/await patterns with comprehensive error handling
- Load tests demonstrate authentication latency remains within acceptable bounds under concurrent request patterns matching production traffic profiles

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline that enforces asynchronous function signatures for all credential operations
- Verified by: Code review checklist that verifies proper async/await usage and promise handling in authentication modules
- Verified by: Integration tests that verify credential operations do not block the event loop and complete within acceptable latency bounds
- Violation handling: Pull requests that introduce synchronous credential operations are automatically blocked by CI pipeline failures
- Violation handling: Code review identifies improper async/await usage and requires corrections before merge approval
- Violation handling: Production monitoring alerts on authentication latency degradation trigger incident response and code audit
- Exception process: Exception requests must document the specific runtime constraints that prevent asynchronous implementation
- Exception process: Security team reviews exception requests to assess cryptographic security implications
- Exception process: Platform architecture team approves exceptions only when alternative architectures are documented and performance impact is measured
- Exception process: Approved exceptions are recorded in the authentication module's architecture documentation with expiration dates for re-evaluation