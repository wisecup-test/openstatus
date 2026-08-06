# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: Key Generation Use

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all API key generation, storage, and verification operations within the authentication subsystem.

## Context

- The system requires secure API key generation for programmatic authentication, where keys serve as bearer credentials for service-to-service or client-to-service communication.
- API keys must be generated with sufficient entropy to resist brute-force attacks and must be stored in hashed form to prevent credential exposure in the event of database compromise.
- The authentication subsystem exposes public contracts for key generation, hashing, verification, and usage tracking, indicating a centralized credential management pattern.
- The implementation uses cryptographic primitives for random byte generation and adaptive hashing algorithms with configurable work factors to balance security and performance.

## Problem Statement

API keys require cryptographically secure generation and storage mechanisms that prevent credential theft, resist offline attacks, and support verification without storing plaintext secrets, while maintaining acceptable performance for authentication operations at scale.

## Decision

1. MUST: API key generation MUST use cryptographic random number generation with at least 128 bits of entropy.

## Policy Block

- MUST API key generation MUST use cryptographic random number generation with at least 128 bits of entropy.

In scope:
- All API key generation operations for programmatic authentication
- All API key storage operations in credential databases
- All API key verification operations during authentication flows
- Usage tracking and auditing mechanisms for API keys

Out of scope:
- User password hashing and verification
- Session token generation and management
- OAuth token handling
- Certificate-based authentication

## Rationale

- The evidence shows explicit use of cryptographic random byte generation and adaptive hashing with configurable work factors, indicating a security-conscious credential management design.
- The public contract structure separating generation, hashing, verification, and usage tracking supports testability and modular composition of authentication workflows.
- Adaptive hashing algorithms with work factors provide defense against offline brute-force attacks while allowing tuning for performance requirements.
- The pattern appears in a database utility module, suggesting centralized credential management that can be consistently applied across the authentication subsystem.

## Consequences

Positive:
- API keys generated with cryptographic randomness resist prediction and brute-force enumeration attacks.
- Hashed storage prevents credential exposure in database breach scenarios, limiting attacker access to computational attack vectors.
- Adaptive hashing with configurable work factors allows security-performance tradeoffs to be tuned as computational capabilities evolve.
- Modular public contracts enable independent testing of generation, storage, and verification logic.

Negative:
- Adaptive hashing introduces computational overhead during authentication, requiring capacity planning for high-throughput scenarios.
- Work factor configuration requires ongoing security review as hardware capabilities increase over time.
- Cryptographic library dependencies introduce supply chain risk and require monitoring for security advisories.
- Lost or compromised keys cannot be recovered from hashed storage, requiring key rotation workflows.

## Alternatives

- Use fast cryptographic hashing without adaptive work factors (rejected)
  Rejected because: Fast hashing algorithms like SHA-256 are vulnerable to GPU-accelerated offline attacks when hashes are compromised, providing insufficient protection for long-lived API credentials.
  When valid: Acceptable for ephemeral tokens with short TTLs where offline attack windows are negligible.
- Store API keys in plaintext with database-level encryption (rejected)
  Rejected because: Database encryption protects data at rest but does not prevent credential exposure if the application layer or database access is compromised, violating defense-in-depth principles.
  When valid: Never valid for bearer credentials; encryption at rest is complementary to hashing, not a substitute.
- Use HMAC-based key derivation with server-side secrets (deferred)
  Rejected because: Not rejected; deferred pending evaluation of key rotation and revocation requirements.
  When valid: Valid when centralized key derivation and instant revocation via secret rotation are architectural requirements.

## Risks

- Cryptographic or hashing library vulnerabilities could compromise all stored credentials if not promptly patched.
  Mitigation: Implement automated dependency scanning, subscribe to security advisories for cryptographic dependencies, and maintain a documented incident response plan for credential rotation.
  Owner: Security Engineering Team
- Insufficient work factor configuration may allow offline attacks to succeed as computational power increases.
  Mitigation: Establish periodic security review cycles to assess work factor adequacy against current hardware capabilities and adjust configuration accordingly.
  Owner: Security Engineering Team
- High authentication throughput may cause performance degradation due to adaptive hashing computational cost.
  Mitigation: Implement performance monitoring for authentication endpoints, establish SLOs for authentication latency, and provision compute capacity based on measured hashing overhead.
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
- Generate random bytes with sufficient length to produce the desired key entropy after encoding; 16 bytes yields 128 bits of entropy before base64 or hex encoding.
- Configure adaptive hashing work factors based on acceptable authentication latency SLOs; measure actual hashing time in production environment to validate configuration.
- Expose verification functions that accept both plaintext tokens and stored hashes to support testing without requiring database access.
- Implement usage tracking with timestamp precision sufficient for audit requirements while avoiding excessive database write load during high-frequency authentication.

## Continuation Context


Verify commands:
- Discover the project's test execution mechanism and run the authentication subsystem test suite to verify key generation produces unique values with expected entropy.
- Discover the project's static analysis or linting configuration and verify that no plaintext API key storage patterns are flagged in credential management code.
- Discover the project's dependency scanning mechanism and verify that cryptographic and hashing libraries are current with no known high-severity vulnerabilities.

Accept when:
- All authentication subsystem tests pass, including key generation uniqueness, hash verification correctness, and constant-time comparison behavior.
- Static analysis confirms no plaintext credential storage in database schemas or ORM models.
- Dependency scans report no high or critical severity vulnerabilities in cryptographic dependencies.

## Enforcement

- Verified by: Automated test suite execution in continuous integration verifying key generation, hashing, and verification contracts.
- Verified by: Static analysis and linting rules detecting plaintext credential storage patterns.
- Verified by: Dependency vulnerability scanning in CI pipeline with build failure on high-severity findings.
- Verified by: Code review checklist requiring verification of cryptographic library usage and work factor configuration.
- Violation handling: CI pipeline failures block merge for test failures, static analysis violations, or dependency vulnerabilities.
- Violation handling: Code review process requires security team approval for changes to credential management logic.
- Violation handling: Runtime monitoring alerts on authentication latency SLO violations indicating potential work factor misconfiguration.
- Violation handling: Incident response procedures trigger credential rotation on detection of plaintext storage or compromised hashing.
- Exception process: Exceptions to cryptographic requirements require written justification and approval from security engineering leadership.
- Exception process: Temporary work factor reductions for performance issues require documented incident tickets with remediation timelines.
- Exception process: All approved exceptions must be recorded in the ADR status log with expiration dates and review triggers.