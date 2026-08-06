# Use Cryptographic Libraries for API Key Generation and Secure Hashing: Consumer Discover Project

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system requires secure generation of API keys for authentication purposes, necessitating cryptographically strong random token generation.
- API keys must be stored securely in the database using one-way hashing to prevent credential exposure in case of data breach.
- The authentication layer exposes public contracts for key generation, hashing, verification, and usage tracking, indicating a standardized credential management interface.
- The codebase uses dedicated cryptographic and password hashing libraries to separate concerns between random token generation and secure storage.

## Problem Statement

API key authentication requires both cryptographically secure token generation and irreversible storage mechanisms to protect credentials at rest while enabling efficient verification during authentication flows.

## Decision

1. MUST: The consumer MUST discover the project's dependency lock file and resolve the exact locked versions of all cryptographic and hashing libraries before implementation.

## Policy Block

- MUST The consumer MUST discover the project's dependency lock file and resolve the exact locked versions of all cryptographic and hashing libraries before implementation.

In scope:
- API key generation for authentication and authorization
- Credential storage in persistent data stores
- API key verification during authentication flows
- Usage tracking for audit and security monitoring

Out of scope:
- Session token generation or JWT signing
- Password hashing for user accounts
- Encryption of data at rest or in transit
- OAuth or third-party authentication flows

## Rationale

- The evidence shows explicit use of cryptographic random byte generation and adaptive cost hashing libraries, indicating a deliberate security architecture for credential management.
- The pattern exposes four distinct public contracts (generateApiKey, hashApiKey, verifyApiKeyHash, shouldUpdateLastUsed), demonstrating separation of concerns between generation, storage, verification, and tracking.
- Using dedicated cryptographic libraries rather than custom implementations follows security best practices and reduces the risk of implementation vulnerabilities.
- The detection of both random generation and hashing libraries in the same module indicates a complete credential lifecycle implementation within a single security boundary.

## Consequences

Positive:
- Cryptographically secure random generation prevents API key prediction and collision attacks.
- Adaptive cost hashing protects stored credentials against brute-force attacks even if the database is compromised.
- Well-defined public contracts enable consistent and auditable credential management across the application.
- Separation between token generation and hashing allows independent security review and testing of each concern.

Negative:
- Adaptive cost hashing introduces computational overhead during authentication, potentially impacting latency under high load.
- Dependency on external cryptographic libraries creates supply chain risk and requires ongoing security monitoring for vulnerabilities.
- The pattern requires careful version management to ensure cryptographic primitives remain secure across library updates.
- Increased complexity in credential lifecycle management compared to simpler but less secure approaches.

## Alternatives

- Use built-in random number generators without cryptographic guarantees for API key generation (rejected)
  Rejected because: Non-cryptographic random generators produce predictable sequences that enable key guessing attacks, violating fundamental security requirements for authentication credentials.
  When valid: Never valid for authentication credentials; only acceptable for non-security-critical identifiers like request tracing IDs.
- Store API keys in plaintext or using reversible encryption in the database (rejected)
  Rejected because: Reversible storage exposes all credentials if the database or encryption keys are compromised, creating a single point of failure for the entire authentication system.
  When valid: Never valid for authentication credentials; one-way hashing is the industry standard for credential storage.
- Implement custom cryptographic primitives instead of using established libraries (rejected)
  Rejected because: Custom cryptographic implementations are highly prone to subtle vulnerabilities and lack the peer review and battle-testing of established libraries.
  When valid: Only valid in specialized cryptographic research contexts or when implementing novel algorithms under expert review.

## Risks

- Cryptographic library vulnerabilities could compromise all stored API keys if exploited before patches are applied.
  Mitigation: Implement automated dependency scanning, subscribe to security advisories for cryptographic libraries, and maintain a rapid patching process for security updates.
  Owner: Security Engineering Team
- Insufficient cost factor configuration for adaptive hashing may allow brute-force attacks on compromised hashes.
  Mitigation: Benchmark authentication latency requirements, configure cost factors based on current hardware capabilities, and implement periodic review of hashing parameters as computing power increases.
  Owner: Engineering Team
- Timing attacks during verification could leak information about stored hash values if constant-time comparison is not properly implemented.
  Mitigation: Use only the constant-time comparison functions provided by the hashing library, and include timing attack tests in the security test suite.
  Owner: Security Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the project's cryptographic library API for random byte generation and determine the appropriate byte length for API keys based on entropy requirements and encoding scheme.
- Locate the hashing library's documentation to identify the cost factor parameter and configure it based on authentication latency benchmarks for the target deployment environment.
- Implement usage tracking with conditional update logic to prevent database write amplification during high-frequency authentication scenarios.
- Ensure all public contracts are exported through a single module interface to maintain a clear security boundary and enable centralized auditing.

## Continuation Context


Verify commands:
- Discover the project's test suite location and execute tests covering API key generation, hashing, verification, and timing attack resistance.
- Locate the project's security scanning configuration and run dependency vulnerability checks against all cryptographic libraries.
- Identify the project's benchmarking tools and measure authentication latency under load to validate cost factor configuration.

Accept when:
- All tests for key generation, hashing, and verification pass with 100% coverage of security-critical code paths.
- Dependency vulnerability scans report no known security issues in cryptographic or hashing libraries.
- Authentication latency benchmarks meet performance requirements while maintaining configured security parameters.

## Enforcement

- Verified by: Automated security testing in continuous integration pipeline
- Verified by: Dependency vulnerability scanning on every build
- Verified by: Code review checklist requiring verification of cryptographic library usage
- Verified by: Periodic security audits of authentication implementation
- Violation handling: Build failures block deployment when security tests fail or vulnerabilities are detected
- Violation handling: Code review rejection for implementations that bypass established cryptographic contracts
- Violation handling: Automated alerts to security team when cryptographic library versions fall behind security patches
- Violation handling: Incident response process for any credential storage violations discovered in production
- Exception process: Exception requests must be submitted to security engineering team with detailed justification
- Exception process: Security architect must review and approve any deviation from cryptographic standards
- Exception process: Approved exceptions require documented compensating controls and time-bound remediation plans
- Exception process: All exceptions are logged and reviewed quarterly for continued validity