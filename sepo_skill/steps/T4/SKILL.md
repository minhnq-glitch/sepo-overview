---
name: sepo-t4
description: "T4 Deploy & UAT — 11 skills: T411-T415 Deploy, T421-T423 UAT, T431-T433 Post-Eval"
---

# Phase T4: Deploy Staging & UAT

## Sub-phases & Skills

### T410: Deploy Staging (5 steps tuần tự)

| Step | Skill | Expert Team | Input | Output |
|------|-------|-------------|-------|--------|
| T411 | Run Migration | DB Architect + DevOps | migration.sql | Schema migrated |
| T412 | Deploy Local | DevOps + QA | Code + .env | App running local |
| T413 | Push Git | DevOps + Git | Code | Code on remote |
| T414 | Deploy Staging | DevOps + SRE | Code on Git | Staging live |
| T415 | Checkpoint STG | Writer + DevOps | Git log + schema | STG checkpoint docs |

### T420: UAT Staging (3 steps tuần tự)

| Step | Skill | Expert Team | Input | Output |
|------|-------|-------------|-------|--------|
| T421 | Chuẩn bị test | QA Lead + Test Data | AP + staging | Test cases + data |
| T422 | UAT feedback | QA + UX | Test cases + staging | TestFeedback.xlsx |
| T423 | Classify A/B/C | QA + PM + Architect | Feedback | Classified list |

### T430: Post-Eval Tasks (3 loại song song)

| Step | Skill | Expert Team | Input | Output |
|------|-------|-------------|-------|--------|
| T431 | A Tasks (UI) | Frontend + UX | A-type feedback | UI fixes |
| T432 | B Tasks (Business) | Backend + BA | B-type feedback | Logic fixes |
| T433 | C Tasks (Perf) | Perf Eng + Architect | C-type feedback | Perf fixes |

## Flow
```
T410: T411 → T412 → T413 → T414 → T415
         ↓
T420: T421 → T422 → T423
         ↓
T430: T431 + T432 + T433 (song song theo A/B/C)
         ↓
Quay lại T410 nếu có fixes → re-deploy
```

## Upload lên SEPO
- T415 → STG_VersionCommit.md, STG_DB_Schema.md
- T422 → TestFeedback_{date}.xlsx
