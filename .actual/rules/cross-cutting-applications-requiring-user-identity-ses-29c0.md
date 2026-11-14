---
version: 2
name: cross-cutting-applications-requiring-user-identity-ses-29c0
description: "Adoption of next-auth for Authentication and Session Management: Applications Requiring User Identity Session Management"
metadata:
  actual:
    adr_id: 29c08eda-8418-4181-bbc7-08869efcdb24
    status: accepted
    candidate_group_id: 3cc28c39-f695-45e8-bfe2-411e18bbbce5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of next-auth for Authentication and Session Management: Applications Requiring User Identity Session Management

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AUTH-001** MUST: Applications requiring user identity, session management, or authentication action handling MUST use next-auth as the standard authentication framework.

### Verify

```bash
manifest=$(find . -maxdepth 3 -name "*manifest*" -o -name "*package*" | head -n 1); [ -n "$manifest" ] && grep -q "next-auth" "$manifest" && echo "next-auth dependency verified in manifest"
grep -rn "next-auth" . --exclude-dir=".*" 2>/dev/null | grep -q "next-auth" && echo "next-auth usage present"
sh -c 'run_tests=$(find . -maxdepth 2 -name "*test*" -o -name "*spec*" | head -n 1); [ -n "$run_tests" ] && echo "Run discovered project test runner on authentication test suites"'
```

**Accept when:**
- The project dependency manifest and lock artifact both resolve next-auth to an active, locked version.
- Authentication routes and server actions route session validation through the centralized authentication module.
- Automated test suites discovered in the repository pass without authentication or type-check regressions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>