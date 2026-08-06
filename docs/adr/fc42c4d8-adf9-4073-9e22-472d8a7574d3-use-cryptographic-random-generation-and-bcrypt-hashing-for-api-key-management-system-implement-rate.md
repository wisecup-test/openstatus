# Use Cryptographic Random Generation and Bcrypt Hashing for API Key Management: System Implement Rate

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all API key generation, storage, and verification operations within the system.

## Context

- The system requires secure API key generation and storage for authentication purposes, as evidenced by dedicated utility functions for key lifecycle management.
- Cryptographic randomness is necessary to prevent predictable token generation that could be exploited through brute-force or pattern-based attacks.
- API keys must be stored in hashed form to protect against credential exposure in the event of database compromise or unauthorized access to storage.
- The authentication flow requires both initial key generation for distribution to clients and subsequent verification against stored hashes without retaining plaintext credentials.
- Time-based usage tracking necessitates efficient hash comparison to avoid performance degradation during high-frequency authentication operations.

## Problem Statement

API keys serve as long-lived credentials that authenticate external clients and services. Storing these credentials in plaintext creates catastrophic risk if the database is compromised. Weak random generation enables prediction attacks. The system must generate cryptographically secure tokens, hash them before storage using adaptive cost functions, and verify them efficiently while maintaining security guarantees and supporting usage analytics.

## Decision

1. SHOULD: The system SHOULD implement rate limiting on verification attempts to mitigate brute-force attacks against stored hashes.

## Policy Block

- SHOULD The system SHOULD implement rate limiting on verification attempts to mitigate brute-force attacks against stored hashes.

In scope:
- All API key generation operations exposed through public contracts
- All API key storage operations that persist credentials
- All API key verification operations during authentication flows
- Utility functions that handle token lifecycle management

Out of scope:
- Session tokens or short-lived JWT credentials with different security models
- User password hashing which may use different cost factors or algorithms
- Internal service-to-service authentication using mutual TLS or other mechanisms
- Development or testing environments where mock authentication is explicitly configured

Exceptions:
- EXC-001: Initial system bootstrap or migration requires one-time plaintext key export for distribution

## Rationale

- The evidence shows explicit use of cryptographic random generation and bcrypt hashing in API key utility functions, indicating a deliberate security architecture for credential management.
- Cryptographic randomness with 128-bit entropy provides sufficient unpredictability to resist brute-force and prediction attacks across the expected keyspace.
- Bcrypt's adaptive cost factor and built-in salt generation provide defense against rainbow table attacks and future-proof the system as computational power increases.
- Separating generation, hashing, and verification into distinct public contracts enables proper security boundaries and supports testing, auditing, and future algorithm migration.

## Consequences

Positive:
- Cryptographically secure random generation eliminates predictability vulnerabilities in API key tokens.
- Bcrypt hashing with adaptive cost protects stored credentials against database compromise and rainbow table attacks.
- Constant-time comparison during verification prevents timing-based side-channel attacks.
- Clear separation of concerns through dedicated utility functions enables security auditing and algorithm upgrades without widespread code changes.

Negative:
- Bcrypt hashing introduces computational cost during both key creation and verification, potentially impacting high-throughput authentication scenarios.
- Adaptive cost factors require periodic review and adjustment as hardware capabilities evolve, adding operational overhead.
- Hash verification cannot be parallelized or cached effectively, limiting horizontal scaling options for authentication services.
- Migration to alternative hashing algorithms requires careful coordination to support both old and new hash formats during transition periods.

## Alternatives

- Use PBKDF2 with HMAC-SHA256 for API key hashing instead of bcrypt (rejected)
  Rejected because: PBKDF2 is more susceptible to GPU-based parallel attacks compared to bcrypt's memory-hard properties, and the evidence explicitly shows bcrypt adoption in the existing implementation.
  When valid: Valid for systems with FIPS 140-2 compliance requirements where bcrypt is not certified, or when integration with existing PBKDF2-based infrastructure is mandatory.
- Store API keys in plaintext and rely on database encryption at rest (rejected)
  Rejected because: Database encryption at rest does not protect against application-level compromise, SQL injection, or unauthorized access by privileged database users. Defense in depth requires hashing credentials before storage.
  When valid: Never valid for production systems. Only acceptable in isolated development environments with synthetic data and no external network access.
- Use Argon2id as the hashing algorithm for enhanced memory-hardness (deferred)
  Rejected because: Not rejected; deferred pending evaluation of library maturity, ecosystem support, and migration complexity from the current bcrypt implementation.
  When valid: Valid for new systems or during planned security upgrades when Argon2id library support is verified and migration testing is completed.

## Risks

- Insufficient bcrypt cost factor allows future hardware advances to enable brute-force attacks against stored hashes within acceptable attacker timeframes.
  Mitigation: Establish annual review cycle for cost factor adequacy. Implement hash upgrade mechanism that transparently re-hashes credentials with higher cost on next successful authentication. Monitor industry recommendations and adjust cost factor to maintain minimum 100ms verification time on current hardware.
  Owner: Security Engineering Team
- Cryptographic library vulnerabilities or implementation flaws could compromise random generation or hashing security guarantees.
  Mitigation: Pin exact library versions through lock files. Subscribe to security advisories for cryptographic dependencies. Implement automated dependency scanning in CI pipeline. Maintain rapid patch deployment process for security updates. Verify library implementations against test vectors from standards bodies.
  Owner: Security Engineering Team and DevOps
- High authentication volume could create CPU bottleneck due to bcrypt computational cost, leading to service degradation or denial of service.
  Mitigation: Implement rate limiting per client and per IP address. Deploy caching layer for recently verified tokens with short TTL. Monitor authentication latency and CPU utilization. Design horizontal scaling strategy for authentication services. Consider async verification queues for non-critical paths.
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
- Discover the project's cryptographic library and verify it provides a secure random byte generation function. Confirm the exact API signature and encoding options in the resolved version's documentation before implementation.
- Discover the project's bcrypt library and verify the hash and compare function signatures. Confirm the cost factor parameter format and valid range for the resolved version. Test hash generation and verification with known inputs before integrating into authentication flows.
- Implement the generation, hashing, and verification functions as separate, testable units with clear contracts. Ensure the generation function returns both the plaintext token for immediate distribution and triggers hashing for storage. Never log or transmit plaintext tokens after initial generation response.
- Design the verification function to return both authentication success status and metadata for usage tracking. Implement constant-time comparison by delegating to the hashing library's built-in compare function rather than manual string comparison.

## Continuation Context


Verify commands:
- Discover the project's test suite location and identify test files covering API key generation, hashing, and verification. Execute the test runner for these specific test suites.
- Discover the project's static analysis or linting configuration. Execute the security-focused linting rules that detect plaintext credential storage or weak random generation patterns.
- Discover the project's dependency scanning tool and execute vulnerability checks against the cryptographic and hashing libraries at their resolved versions.

Accept when:
- All test suites for API key generation, hashing, and verification pass with 100% success rate and demonstrate correct handling of valid tokens, invalid tokens, and edge cases.
- Static analysis confirms no plaintext API key storage in persistent layers and no use of non-cryptographic random generation for token creation.
- Dependency scanning reports no known vulnerabilities in cryptographic or hashing libraries at the resolved versions, or documented exceptions exist with mitigation plans.

## Enforcement

- Verified by: Automated test suite execution in continuous integration pipeline covering generation, hashing, and verification functions with security-focused test cases.
- Verified by: Static code analysis scanning for plaintext credential storage patterns and weak random generation usage.
- Verified by: Security-focused code review checklist requiring verification of cryptographic library usage and hash parameter configuration.
- Verified by: Dependency vulnerability scanning integrated into CI pipeline with blocking failures on high-severity cryptographic library vulnerabilities.
- Violation handling: CI pipeline failures block merge for test failures, static analysis violations, or dependency vulnerabilities in cryptographic libraries.
- Violation handling: Code review process requires security team approval for any changes to API key generation, hashing, or verification logic.
- Violation handling: Runtime monitoring alerts on authentication failures exceeding threshold rates, indicating potential brute-force attacks or implementation issues.
- Violation handling: Incident response process triggers for any detection of plaintext API key storage or weak random generation in production systems.
- Exception process: Exception requests must be submitted to security team with documented justification, risk assessment, and compensating controls.
- Exception process: Security team reviews exception requests within 2 business days and approves only with time-bound expiration and remediation plan.
- Exception process: Approved exceptions are documented in security exception register with owner, expiration date, and quarterly review requirement.
- Exception process: All exceptions require re-approval upon expiration and cannot be automatically renewed without fresh risk assessment.