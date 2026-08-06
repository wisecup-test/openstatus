# Store API Key Credentials Using Salted Hash Functions: Token Verification Use

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- API keys serve as bearer tokens for programmatic access, requiring secure storage to prevent credential compromise if the database is breached.
- The system generates random tokens for API keys and must verify them against stored values during authentication without storing plaintext credentials.
- Cryptographic hash functions with salt provide one-way transformation of credentials, allowing verification while preventing reverse computation of the original token.
- The authentication module exports public contracts for key generation, hashing, verification, and usage tracking, establishing a boundary between credential management and application logic.

## Problem Statement

API keys must be stored securely in the database such that compromise of the storage layer does not expose plaintext credentials, while still enabling efficient verification of presented tokens during authentication requests.

## Decision

1. MUST: Token verification MUST use constant-time comparison functions provided by the hashing library to prevent timing attacks.

## Policy Block

- MUST Token verification MUST use constant-time comparison functions provided by the hashing library to prevent timing attacks.

In scope:
- All API key credential storage operations
- Token generation for new API keys
- Authentication verification flows that validate API keys
- Database persistence of hashed credentials

Out of scope:
- Session tokens or JWT credentials
- User password storage mechanisms
- OAuth or third-party authentication flows
- API key transmission protocols or transport security

## Rationale

- The IR evidence shows explicit use of salted hash functions for credential storage and constant-time comparison for verification, indicating a deliberate security posture against credential compromise.
- Random byte generation with 16-byte entropy provides sufficient randomness for bearer tokens while the hash function work factor balances security against computational cost.
- Separation of concerns through public contracts for generation, hashing, and verification enables independent testing and future algorithm migration without coupling to application logic.
- The pattern appears in a database utility module with 91% significance, suggesting this is an established architectural boundary rather than ad-hoc implementation.

## Consequences

Positive:
- Database breach does not expose plaintext API keys, limiting the blast radius of storage-layer compromise.
- Constant-time comparison prevents timing-based attacks that could leak information about stored hashes.
- Modular public contracts enable independent testing of cryptographic operations and future algorithm upgrades.
- Configurable work factor allows tuning of computational cost versus security strength as threat models evolve.

Negative:
- Hash computation adds latency to both credential creation and verification operations, impacting authentication performance.
- Lost or forgotten API keys cannot be recovered and must be regenerated, requiring user notification and key rotation workflows.
- Work factor tuning requires careful benchmarking to balance security against denial-of-service risk from expensive hash operations.
- Dependency on external cryptographic libraries introduces supply-chain risk and requires vigilant version management.

## Alternatives

- Store API keys in plaintext or using reversible encryption (rejected)
  Rejected because: Plaintext or reversible encryption exposes all credentials if the database or encryption key is compromised, violating defense-in-depth principles.
  When valid: Never valid for bearer tokens with database persistence.
- Use fast cryptographic hash functions without salt or work factor (rejected)
  Rejected because: Unsalted or fast hashes are vulnerable to rainbow table attacks and GPU-accelerated brute force, providing insufficient protection against offline attacks.
  When valid: Only when credentials are ephemeral and never persisted beyond a single request lifecycle.
- Delegate credential storage to external secrets management service (deferred)
  Rejected because: Introduces operational complexity and external dependency, though may be appropriate for high-security environments.
  When valid: When compliance requirements mandate hardware security modules or when credential volume exceeds database scaling limits.

## Risks

- Cryptographic library vulnerabilities could compromise all stored credentials if hash function is broken or implementation contains side-channel leaks.
  Mitigation: Pin exact dependency versions in lock file, subscribe to security advisories, and implement automated vulnerability scanning in CI pipeline.
  Owner: Security Engineering Team
- Insufficient work factor or entropy could allow offline brute-force attacks if database is compromised and computational resources are available to attackers.
  Mitigation: Benchmark work factor against current hardware capabilities, monitor cryptographic best practices, and plan for periodic work factor increases as compute costs decline.
  Owner: Engineering Team
- Hash computation cost could enable denial-of-service attacks through repeated authentication attempts with invalid credentials.
  Mitigation: Implement rate limiting on authentication endpoints, monitor authentication latency metrics, and consider adaptive work factors based on request patterns.
  Owner: Platform Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the database utility module that exports credential management contracts and verify that hash, compare, and random byte generation functions are imported from the resolved cryptographic dependencies.
- Ensure that generated tokens are presented to the user exactly once during creation and that only the hashed value is persisted to the database schema.
- Implement usage tracking logic that compares current timestamp against last-used timestamp to determine whether an update is necessary, avoiding unnecessary write operations on every authentication.

## Continuation Context


Verify commands:
- Discover the project's test suite location and execute the authentication module tests to verify hash generation and verification behavior.
- Locate the project's static analysis or linting configuration and run security-focused rules to detect plaintext credential storage patterns.
- Identify the project's dependency audit tooling and execute vulnerability scans against the resolved cryptographic library versions.

Accept when:
- All authentication module tests pass, including hash generation, verification with valid tokens, rejection of invalid tokens, and constant-time comparison behavior.
- Static analysis reports no plaintext credential storage violations and no use of deprecated or insecure cryptographic functions.
- Dependency audit shows no known vulnerabilities in cryptographic libraries at the resolved versions, or documented exceptions exist for accepted risks.

## Enforcement

- Verified by: Automated test suite execution in continuous integration pipeline covering credential generation, hashing, and verification flows.
- Verified by: Static analysis and security linting rules that detect plaintext credential patterns or use of weak cryptographic primitives.
- Verified by: Dependency vulnerability scanning that fails builds when cryptographic libraries have known security issues.
- Verified by: Code review checklist requiring verification that API key storage follows hashing requirements and that plaintext tokens are never persisted.
- Violation handling: CI pipeline fails and blocks merge if authentication tests fail or static analysis detects credential storage violations.
- Violation handling: Security scanning failures trigger immediate notification to security team and block deployment until resolved or exception granted.
- Violation handling: Code review identifies violations before merge, requiring rework to comply with hashing requirements.
- Violation handling: Runtime monitoring alerts on authentication latency anomalies that may indicate work factor misconfiguration or denial-of-service attempts.
- Exception process: Exception requests must document the specific requirement that conflicts with the hashing policy and propose alternative compensating controls.
- Exception process: Security engineering team reviews exception requests and assesses risk impact, approving only when compensating controls provide equivalent protection.
- Exception process: Approved exceptions are time-bound and require periodic re-evaluation as threat landscape and cryptographic best practices evolve.
- Exception process: All exceptions are recorded in a central registry with rationale, approval authority, expiration date, and required compensating controls.