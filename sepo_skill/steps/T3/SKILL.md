---
name: sepo-t3
description: "T3 Build — 4 skills: T310 Build Prompt, T320 Test Prompt, T380 Finish ISP, T390 Eval"
---

# Phase T3: Build BApp

## Skills trong phase này

| Step | Skill | Expert Team | Input | Output |
|------|-------|-------------|-------|--------|
| T310-001 | Build Prompt | Senior Eng + Reviewer | ISP step + AP | Code + Tests |
| T320-001 | Test Prompt | QA + Security | ISP step + code | Test results |
| T380 | Hoàn thành ISP | DevOps + Writer | All code | ISP updated, code pushed |
| T390 | Eval vs AP | Architect + Auditor + QA | AP + code | PostAudit report |

## Flow
```
┌─────────────────────────────────────┐
│  Lặp mỗi ISP step:                 │
│  T310-001 (build) → T320-001 (test)│
│       ↓ pass? → next step           │
│       ↓ fail? → fix → retry         │
└─────────────────────────────────────┘
         ↓ all steps done
T380 (finish ISP) → T390 (eval vs AP)
```

## Download từ SEPO
- T310: ISP step hiện tại + AP section liên quan
- T320: Code vừa build + ISP test criteria
- T390: AP + toàn bộ codebase

## Upload lên SEPO
- T390 → PostAudit_{version}.md
