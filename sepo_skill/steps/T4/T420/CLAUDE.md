# CLAUDE.md — T420: UAT Staging (L2: Sub-phase)

> **Level:** L2 — UAT sub-phase of T4.
> **Steps:** T421→T422→T423 (sequential)

## Flow
```
T421: Prepare test data + write test cases from AP
  ↓
T422: Execute UAT on staging, record bugs in TestFeedback template
  ↓
T423: Classify each bug/feedback as A (UI) / B (Business) / C (Performance)
```

## Rule: All test cases must be executed. No skipping.
## Rule: Critical bugs BLOCK deployment to production.
