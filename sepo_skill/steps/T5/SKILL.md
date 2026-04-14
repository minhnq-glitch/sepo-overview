---
name: sepo-t5
description: "T5 Production — 2 skills: T510 Deploy Prod, T530 Checkpoint PRD"
---

# Phase T5: Production

## Skills trong phase này

| Step | Skill | Expert Team | Input | Output |
|------|-------|-------------|-------|--------|
| T510 | Deploy Prod | DevOps + SRE + DB | Staging verified | Production live |
| T530 | Checkpoint PRD | Writer + DevOps | Git log + schema | PRD checkpoint docs |

## Flow
```
T510 (deploy production) → T530 (checkpoint PRD) → DONE!
```

## Preconditions
- UAT passed (T420)
- All critical bugs fixed (T430)
- Staging verified

## Upload lên SEPO
- T530 → PRD_VersionCommit.md, PRD_DB_Schema.md
