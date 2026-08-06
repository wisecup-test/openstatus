# Use Asynchronous API Key Generation and Hashing for Credential Management: Key Generation Operations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all API key generation, hashing, verification, and credential storage operations within the data protection domain.

## Context

- The system requires secure API key generation and storage for authentication, using cryptographically secure random byte generation and bcrypt-based hashing
- API key operations are exposed through public contracts including generation, hashing, verification, and last-used timestamp logic
- The implementation uses asynchronous functions for key generation, hashing, and verification to prevent blocking operations during cryptographic computations
- Credential storage relies on bcrypt with a cost factor of 10 for password hashing, balancing security and performance

## Problem Statement

API key generation and credential hashing operations are computationally expensive and can block the event loop if executed synchronously, degrading system responsiveness and throughput under concurrent authentication load.

## Decision

1. MUST: All API key generation operations MUST use cryptographically secure random byte generation with a minimum entropy of 128 bits

## Policy Block

- MUST All API key generation operations MUST use cryptographically secure random byte generation with a minimum entropy of 128 bits

In scope:
- All API key generation operations
- All credential hashing and verification operations
- All authentication token storage mechanisms
- All cryptographic random number generation for security purposes

Out of scope:
- Session token generation using non-cryptographic randomness
- Client-side credential validation
- Third-party OAuth token handling
- Non-authentication related hashing operations

Exceptions:
- EXC-001: Synchronous hashing is permitted in test environments where blocking behavior is acceptable and performance is not critical

## Rationale

- Asynchronous cryptographic operations prevent blocking the event loop during expensive bcrypt hashing and random byte generation, maintaining system responsiveness under concurrent load
- The evidence shows explicit use of crypto.randomBytes for secure random generation and bcrypt for credential hashing, establishing a clear pattern of cryptographic best practices
- Public contracts for generateApiKey, hashApiKey, verifyApiKeyHash, and shouldUpdateLastUsed indicate a deliberate architectural choice to centralize and standardize credential management
- The bcrypt cost factor of 10 provides a reasonable balance between security and performance for modern hardware

## Consequences

Positive:
- Non-blocking cryptographic operations maintain system responsiveness during concurrent authentication requests
- Centralized public contracts ensure consistent security practices across all API key operations
- Cryptographically secure random generation provides high-entropy API keys resistant to prediction attacks
- Bcrypt hashing with appropriate cost factor protects stored credentials against brute-force attacks

Negative:
- Asynchronous operations introduce complexity in error handling and require proper promise or async/await management
- Bcrypt hashing operations consume significant CPU resources even when non-blocking, potentially affecting overall system capacity
- The cost factor of 10 may require adjustment over time as hardware capabilities evolve

## Alternatives

- Use synchronous cryptographic operations for simpler code flow (rejected)
  Rejected because: Synchronous bcrypt hashing blocks the event loop for 50-100ms per operation, causing severe performance degradation under concurrent authentication load
  When valid: Only acceptable in single-threaded batch processing contexts where blocking is not a concern
- Use faster hashing algorithms like SHA-256 instead of bcrypt (rejected)
  Rejected because: Fast hashing algorithms are vulnerable to brute-force attacks; bcrypt's computational cost is a security feature that protects against credential cracking
  When valid: Acceptable for non-credential hashing such as data integrity checks or cache keys
- Offload cryptographic operations to a dedicated worker pool (deferred)
  Rejected because: Adds architectural complexity with worker management and inter-process communication overhead
  When valid: Consider if profiling shows cryptographic operations consuming excessive CPU time despite async implementation

## Risks

- Unhandled promise rejections in asynchronous cryptographic operations could lead to silent authentication failures
  Mitigation: Implement comprehensive error handling with logging and monitoring for all async credential operations
  Owner: Engineering team
- The bcrypt cost factor of 10 may become insufficient as hardware capabilities increase over time
  Mitigation: Establish periodic security reviews to assess and adjust the cost factor based on current hardware benchmarks
  Owner: Security team
- Concurrent high-volume authentication requests could still cause CPU saturation despite non-blocking operations
  Mitigation: Implement rate limiting on authentication endpoints and monitor CPU utilization metrics
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure all cryptographic functions are properly awaited and wrapped in try-catch blocks to handle potential errors from the underlying cryptographic libraries
- Consider implementing a credential rotation strategy that allows gradual migration if the bcrypt cost factor needs to be increased
- Monitor authentication endpoint latency to detect performance degradation that might indicate the need for worker pool offloading

## Continuation Context


Verify commands:
- Discover the project's test execution mechanism and run the test suite covering API key generation, hashing, and verification operations
- Discover the project's static analysis configuration and execute type checking to verify all cryptographic functions return promises
- Discover the project's linting configuration and verify no synchronous cryptographic operations are used in production code paths

Accept when:
- All API key generation, hashing, and verification functions are implemented as async functions and properly awaited at call sites
- Static analysis confirms no blocking cryptographic operations exist in production code paths
- Test suite demonstrates that concurrent authentication requests do not block the event loop

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline checking for synchronous cryptographic operations
- Verified by: Code review checklist requiring verification of async/await patterns for all credential operations
- Verified by: Performance testing suite measuring authentication endpoint latency under concurrent load
- Violation handling: CI pipeline fails if synchronous cryptographic operations are detected in production code paths
- Violation handling: Code review blocks merge requests that introduce blocking credential operations
- Violation handling: Performance regression alerts trigger investigation if authentication latency exceeds baseline thresholds
- Exception process: Request exception through security team lead with documented justification
- Exception process: Provide evidence that the exception scope is limited to non-production or test-only contexts
- Exception process: Document the exception in code comments and architectural decision log