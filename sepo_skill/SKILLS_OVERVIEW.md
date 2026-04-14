# SEPO Skills Overview — Tổng hợp toàn bộ Skills theo tầng

## Tầng 0: Master Skill

| File | Mô tả |
|------|-------|
| `SKILL.md` | **Master Orchestrator** — Detect step → Download docs từ SEPO → Execute skill con → Upload output |

---

## Tầng 1: Phase Skills (6 phases)

Mỗi phase là 1 file `SKILL.md` trong folder phase.

| Phase | File | Expert Teams chính | Số skills con |
|-------|------|---------------------|---------------|
| **T0** Setup | `steps/T0/SKILL.md` | DevOps + Backend Dev | 1 |
| **T1** Vẽ Voi | `steps/T1/SKILL.md` | BA + Architect + UX + PM | 4 |
| **T2** AP & ISP | `steps/T2/SKILL.md` | Architect + DB + Writer + Program Lead + QA | 4 |
| **T3** Build | `steps/T3/SKILL.md` | Senior Eng + QA + Security + Auditor | 4 |
| **T4** Deploy & UAT | `steps/T4/SKILL.md` | DevOps + SRE + DB + QA + Frontend + Backend | 11 |
| **T5** Production | `steps/T5/SKILL.md` | DevOps + SRE + DB + Writer | 2 |

---

## Tầng 2: Step Skills (38 steps)

### T0: Setup môi trường

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T010 | `T0/T010.md` | DevOps Engineer + Backend Developer | GIT-DB-Structure.md, vault.md | Repos cloned, DB connected, .env |

---

### T1: Vẽ Voi (System Analysis)

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T110 | `T1/T110.md` | Business Analyst + System Architect | VOI/requirement docs | L1_VoiTong.md |
| T120 | `T1/T120.md` | Business Analyst + Process Designer | L1_VoiTong.md | L2_Workflows.md |
| T130 | `T1/T130.md` | BA + UX Designer + Backend Dev | L1 + L2 docs | L3_WorkflowDetails.md |
| T140 | `T1/T140.md` | Technical Writer + PM | L1 + L2 + L3 | ProjectGuide.md |

---

### T2: AP & ISP

**Sub-phase T210: Dựng ArchitecturePack** (`T2/T210/SKILL.md`)

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T211 | `T2/T210/T211.md` | Database Architect + Backend Developer | L1/AP draft | migration.sql, ERD |
| T212 | `T2/T210/T212.md` | Software Architect + Tech Writer + DB Architect | L1, L2, L3, schema, layerevent.md | ArchitecturePack.md (7 sections) |

**Direct steps:**

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T220 | `T2/T220.md` | Tech Program Lead + Architect + QA Lead | ArchitecturePack | IncrementalStepPlan.md |
| T230 | `T2/T230.md` | ISP Quality Auditor + Architect | ISP + AP | IncrementalStepPlanAudited.md |

---

### T3: Build BApp

**Sub-phase T300-001: Build per step (lặp cho mỗi ISP step)** (`T3/T300-001/SKILL.md`)

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T310-001 | `T3/T300-001/T310-001.md` | Senior Engineer + Code Reviewer | ISP step + AP section | Code + tests passing |
| T320-001 | `T3/T300-001/T320-001.md` | QA Engineer + Security Tester | ISP step + code | Test results + known limitations |

**Direct steps:**

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T380 | `T3/T380.md` | DevOps Engineer + Technical Writer | All code | ISP updated, code pushed |
| T390 | `T3/T390.md` | Software Architect + Code Auditor + QA Lead | AP + full codebase | PostAudit.md |

---

### T4: Deploy & UAT

**Sub-phase T410: Deploy Staging (5 steps tuần tự)** (`T4/T410/SKILL.md`)

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T411 | `T4/T410/T411.md` | Database Architect + DevOps | migration.sql | Schema migrated |
| T412 | `T4/T410/T412.md` | DevOps Engineer + QA Engineer | Code + .env + migrated DB | App running local, smoke tests pass |
| T413 | `T4/T410/T413.md` | DevOps Engineer + Git Specialist | Local tested code | Code on remote, CI passed |
| T414 | `T4/T410/T414.md` | DevOps Engineer + SRE | Code on Git | Staging live, health OK |
| T415 | `T4/T410/T415.md` | Technical Writer + DevOps | Git log + DB schema | STG_VersionCommit.md, STG_DB_Schema.md |

**Sub-phase T420: UAT Staging (3 steps tuần tự)** (`T4/T420/SKILL.md`)

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T421 | `T4/T420/T421.md` | QA Lead + Test Data Engineer | AP + staging URL | TestCases.md + test data |
| T422 | `T4/T420/T422.md` | QA Tester + UX Reviewer | Test cases + staging + T720_Template | TestFeedback_{date}.xlsx |
| T423 | `T4/T420/T423.md` | QA Lead + PM + Software Architect | TestFeedback + layerevent.md | Classified feedback list |

**Sub-phase T430: Post-Eval Tasks (3 loại song song)** (`T4/T430/SKILL.md`)

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T431 | `T4/T430/T431.md` | Frontend Developer + UX Designer | A-type feedback | UI fixes |
| T432 | `T4/T430/T432.md` | Backend Developer + Business Analyst | B-type feedback | Business logic fixes |
| T433 | `T4/T430/T433.md` | Performance Engineer + Architect | C-type feedback | Performance optimizations |

---

### T5: Production

| Step | File | Expert Team | Input | Output |
|------|------|-------------|-------|--------|
| T510 | `T5/T510.md` | DevOps + SRE + Database Architect | Staging verified | Production live |
| T520 | `T5/T520.md` | — (umbrella) | — | — |
| T530 | `T5/T530.md` | Technical Writer + DevOps | Git log + Prod schema | PRD_VersionCommit.md, PRD_DB_Schema.md |

---

## Flow Execution

```
USER INPUT
    │
    ▼
┌─────────────────────────────────────┐
│ [MASTER] SKILL.md                   │
│  - Detect step từ keyword/context   │
│  - Check local workspace            │
│  - Download docs từ SEPO nếu thiếu  │
└─────────┬───────────────────────────┘
          │
          ▼
┌─────────────────────────────────────┐
│ [L1 PHASE] T{phase}/SKILL.md        │
│  - Mô tả phase, list skills con     │
│  - Cho biết flow trong phase        │
└─────────┬───────────────────────────┘
          │
          ▼
┌─────────────────────────────────────┐
│ [L2 STEP] T{phase}/T{step}.md       │
│  - 18 sections đầy đủ               │
│  - Expert team định sẵn             │
│  - Execution Procedure step-by-step │
│  - Reusable Prompt Block (prompt    │
│    gốc từ SEPO Web)                 │
└─────────┬───────────────────────────┘
          │
          ▼
    EXECUTE + UPLOAD OUTPUT
```

---

## Tổng kê

| Tầng | Loại | Số lượng |
|------|------|----------|
| L0 | Master | 1 (SKILL.md) |
| L1 | Phase | 6 (T0-T5) + 5 sub-phase (T210, T300-001, T410, T420, T430) = **11** |
| L2 | Step | **38** (26 có prompt đầy đủ + 12 stub) |
| **Total** | | **50 files** |

Expert teams:
- **DevOps/SRE**: T010, T380, T411, T412, T413, T414, T415, T510, T530
- **Business Analyst**: T110, T120, T130, T432
- **Architect**: T110, T212, T220, T230, T390, T423, T433
- **Database Architect**: T211, T212, T411, T510
- **QA/Tester**: T230, T320-001, T390, T421, T422, T423
- **Frontend/UX**: T130, T431
- **Senior Engineer**: T310-001, T320-001
- **Technical Writer**: T140, T212, T415, T530
