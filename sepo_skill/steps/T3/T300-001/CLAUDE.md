# CLAUDE.md — T300-001: Build BApp Per Step (L2: Sub-phase)

> **Level:** L2 — Build loop sub-phase of T3.
> **Steps:** T310-001 (Build) → T320-001 (Test) — repeat per ISP step.

## Loop Flow
```
For each ISP step:
  T310-001: Write tests → implement → verify
    ↓ pass?
  T320-001: Edge cases → security → report
    ↓ pass?
  Next ISP step
    ↓ all done?
  T380: Finish ISP
```

## Critical: NEVER skip T320 (test). Every build step MUST be tested.
