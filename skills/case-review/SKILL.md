---
name: case-review
description: Evidence graph review (route R10). Read-only validation of a reverse-skill case before handoff: scope readiness, Evidence records, Finding references, artifact hash fixity. Use before finishing any case or when the owner asks to review/audit case evidence.
---

# Case review (R10)

## When to use
Before any handoff (checklist gate), or when the owner asks to review a case /
证据链 / scope readiness. Read-only — never modifies the case.

## Workflow (ACTION REQUIRED)
1. Run the validator:

```bash
python3 skills/case-review/scripts/review_case.py work/<case> --verify-hashes --strict
```

2. What it checks (stdlib-only python):
   - scope.md: meta fields, auth.status, network_profile, ready_for_act
   - evidence/: E-* records, required fields, referenced artifacts exist and
     sha256 matches (fixity)
   - workitems.md / timeline.md: referenced IDs resolve
   - findings: every Finding cites ≥1 Evidence
   - paths: every Path references a Finding
3. Fix what it flags (in the case files), re-run until clean.
4. If the owner asked for a *review report*: summarize gate status, gaps, and
   recommended fixes — do not fix without being asked.

## Boundaries
- Read-only tool: review_case.py writes nothing.
- This module never expands scope or authorizes actions.
