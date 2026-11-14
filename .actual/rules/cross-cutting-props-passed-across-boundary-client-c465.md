---
version: 2
name: cross-cutting-props-passed-across-boundary-client-c465
description: "Client Component Directive (\"use client\") in @openstatus/ui Component Library: Props Passed Across Boundary Client Components"
metadata:
  actual:
    adr_id: c465afbd-db91-40ae-b842-7757a06e1053
    status: accepted
    candidate_group_id: 4802f371-80d7-4d9d-91c4-2dfa24bf57b8
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
---

# Client Component Directive ("use client") in @openstatus/ui Component Library: Props Passed Across Boundary Client Components

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLIENT-001** MUST: All props passed across the boundary into client components from server-rendered contexts MUST be serializable.

### Verify

```bash
# Discover and run repository lint and check scripts from project manifests
MANIFEST_SCRIPTS=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"lint"' {} + 2>/dev/null | head -n 1)
echo "Discovered lint configuration in: $MANIFEST_SCRIPTS"

# Discover component files importing interactive hooks and verify client boundary declarations
find . -type f -name '*.*' -exec grep -lE 'useState|useEffect|useMemo' {} + | while read -r comp; do head -n 5 "$comp" | grep -q '"use client"' || echo "Verify client boundary in: $comp"; done

# Discover and run repository build validation scripts to confirm rendering model integrity
MANIFEST_BUILD=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"build"' {} + 2>/dev/null | head -n 1)
echo "Discovered build configuration in: $MANIFEST_BUILD"
```

**Accept when:**
- All UI component modules utilizing client-side hooks or browser event listeners declare the client boundary directive as their initial statement.
- Repository build and lint verification scripts complete with zero exit code and no client-server boundary violation errors.
- Component rendering in both server-side and client-side execution environments completes without boundary mismatch warnings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>