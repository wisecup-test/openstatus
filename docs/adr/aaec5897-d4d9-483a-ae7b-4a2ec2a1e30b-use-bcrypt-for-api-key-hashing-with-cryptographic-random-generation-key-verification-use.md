# Use Bcrypt for API Key Hashing with Cryptographic Random Generation: Key Verification Use

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system generates API keys for authentication and must store them securely to prevent credential compromise
- API keys are long-lived credentials that require protection against offline brute-force attacks if the database is compromised
- The codebase uses cryptographic random generation for token creation and adaptive hashing for storage, separating generation from verification concerns
- A utility module exports public contracts for API key lifecycle operations including generation, hashing, verification, and usage tracking

## Problem Statement

API keys must be generated with sufficient entropy and stored in a manner that resists offline attacks, while supporting efficient verification during authentication and tracking of usage patterns for security monitoring.

## Decision

1. MUST: API key verification MUST use constant-time comparison through the hashing library's verification function

## Policy Block

- MUST API key verification MUST use constant-time comparison through the hashing library's verification function

In scope:
- API key generation for authentication purposes
- API key storage in persistent data stores
- API key verification during authentication flows
- API key usage tracking and last-used timestamp updates

Out of scope:
- User password hashing, which may have different work factor requirements
- Session tokens or JWT signing, which use different cryptographic primitives
- OAuth tokens or third-party authentication credentials
- Encryption of data at rest beyond credential storage

Exceptions:
- EXC-001: Development and testing environments may use reduced work factors to improve test suite performance

## Rationale

- The evidence shows explicit use of cryptographic random generation and adaptive hashing with a work factor of 10, indicating a deliberate security posture for credential storage
- Separating generation, hashing, and verification into distinct public contracts enables modular security testing and supports the principle of least privilege in code organization
- The pattern uses industry-standard adaptive hashing to protect against GPU-accelerated brute-force attacks while maintaining acceptable verification performance
- Usage tracking through last-used timestamps provides security monitoring capabilities without compromising the hashed credential storage

## Consequences

Positive:
- API keys are protected against offline brute-force attacks even if the database is compromised
- Cryptographic random generation ensures sufficient entropy to prevent prediction or collision attacks
- Modular public contracts enable independent testing of generation, storage, and verification logic
- Adaptive hashing allows future work factor increases as computational power grows

Negative:
- Adaptive hashing introduces computational cost during authentication, potentially impacting high-throughput API scenarios
- Work factor tuning requires balancing security against performance, necessitating periodic review
- The pattern requires careful dependency management to ensure consistent hashing behavior across deployments
- Usage tracking adds database write operations to the authentication path, increasing transaction complexity

## Alternatives

- Use HMAC-SHA256 with a server-side secret for API key hashing (rejected)
  Rejected because: HMAC is fast and does not provide adaptive work factor protection against brute-force attacks; compromise of the server secret exposes all API keys
  When valid: Acceptable only when API keys are short-lived and rotated frequently, or when hardware security modules protect the HMAC key
- Store API keys encrypted with application-level encryption keys (rejected)
  Rejected because: Encryption is reversible and requires secure key management; compromise of the encryption key exposes all API keys in plain text
  When valid: Valid for scenarios requiring key recovery or rotation, but must be combined with access controls and hardware security modules
- Use Argon2id as the adaptive hashing algorithm instead of bcrypt (deferred)
  Rejected because: Not rejected; Argon2id provides memory-hard properties that further resist GPU attacks, but requires evaluation of library maturity and deployment constraints
  When valid: Valid for new implementations or when migrating credentials; requires careful work factor and memory parameter tuning

## Risks

- Work factor of 10 may become insufficient as computational power increases, allowing faster brute-force attacks
  Mitigation: Establish annual review cycle for work factor adequacy; implement versioned hashing to support gradual migration to higher work factors
  Owner: Security team
- Dependency vulnerabilities in the adaptive hashing library could compromise credential security
  Mitigation: Enable automated dependency scanning in CI pipeline; subscribe to security advisories for the hashing library; maintain update process for security patches
  Owner: Engineering team
- Timing attacks during verification could leak information about stored hashes if constant-time comparison is not enforced
  Mitigation: Use only the hashing library's built-in verification function; add security testing to verify constant-time behavior; document prohibition of custom comparison logic
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
- Generate API keys by obtaining random bytes from the cryptographic module and encoding them in a URL-safe format; return both the plain token to the client and the hashed version for storage
- Implement usage tracking with conditional updates to avoid unnecessary database writes; consider a time threshold to update last-used timestamps only when sufficient time has elapsed
- Separate the hashing and verification logic into distinct functions to enable independent unit testing with known test vectors and performance benchmarking

## Continuation Context


Verify commands:
- Discover the project's test execution configuration and run the test suite covering API key generation, hashing, and verification to confirm cryptographic properties
- Discover the project's static analysis or linting configuration and execute security-focused rules to detect plain-text credential storage or weak random generation
- Discover the project's dependency scanning configuration and verify that the adaptive hashing library is present at the locked version with no known vulnerabilities

Accept when:
- All tests for API key generation produce tokens with sufficient entropy and all hashing tests verify work factor compliance
- Static analysis reports no instances of plain-text credential storage or use of non-cryptographic random generation
- Dependency scanning confirms the hashing library is at the locked version with no high or critical severity vulnerabilities

## Enforcement

- Verified by: Automated unit and integration tests in continuous integration pipeline
- Verified by: Static analysis and security scanning tools integrated into pre-commit and CI workflows
- Verified by: Code review checklist requiring verification of cryptographic random generation and adaptive hashing usage
- Verified by: Periodic security audits reviewing credential storage implementation and work factor adequacy
- Violation handling: CI pipeline fails if tests detect weak random generation or missing adaptive hashing
- Violation handling: Code review blocks merge if plain-text credential storage is detected
- Violation handling: Security scanning alerts trigger immediate investigation and remediation
- Violation handling: Production incidents involving credential compromise trigger security incident response protocol
- Exception process: Exception requests must document specific technical constraints preventing compliance
- Exception process: Security team review and approval required for all exceptions
- Exception process: Approved exceptions must include compensating controls and time-bound remediation plan
- Exception process: Exception registry maintained with periodic review to ensure temporary exceptions do not become permanent