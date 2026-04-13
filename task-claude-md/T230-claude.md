# CLAUDE.md — Task T230: Audit Incremental Step Plans

## Task Identity
- **Code:** T230
- **Name:** Audit ISP
- **Phase:** T2 — Architecture Pack & ISP
- **Classification:** Type A
- **Expert Team:** ISP Quality Auditor + Software Architect

## Objective
Kiem tra chat luong ISP: coverage, clarity, test criteria, va enrich voi data/API/UI details. Day la **Quality Gate #1** — ISP phai pass 7 tieu chi truoc khi code.

## Preconditions
- ISP (T220) da tao
- AP da approved

## Input Docs (Required)
- **IncrementalStepPlan.md** — ISP can audit
- **ArchitecturePack.md** — Source of truth cho business logic
- **Clevai_Tech_Stack_Prefer.md** — Tech stack preferences

## Output Contract
- **IncrementalStepPlanAudited.md**:
  - Moi step da enriched voi chi tiet
  - Coverage matrix: AP section -> ISP step
  - Gaps identified va resolved
  - Verdict per step: **ENRICH** (ok) hoac **BLOCK** (can fix)

## Instructions

Ban la ISP Quality Auditor. Nhiem vu: kiem tra tung ISP step co du detail de mot AI coding agent (Claude Code) implement chinh xac hay khong.

### 7 Audit Criteria per Step:

| # | Criteria | Check |
|---|---------|-------|
| 1 | **Data Source** | Table name, field names cu the? |
| 2 | **API Contract** | Method, endpoint, request/response schema? |
| 3 | **UI Specification** | Wireframe reference, layout chi tiet? |
| 4 | **Business Logic** | Formulas, algorithms, conditions cu the? |
| 5 | **Acceptance Criteria** | Checklist-based, cover FE + BE? |
| 6 | **Library/Tool** | Library cu the (e.g., "use excelize for Excel")? |
| 7 | **Dependencies** | Step truoc nao phai xong? |

### Verdict Rules:
- **ENRICH:** >= 5/7 criteria co, phan thieu co the suy ra
- **BLOCK:** < 5/7 criteria co, hoac thieu critical info (data source, API)
- Neu BLOCK: ghi ro can bo sung gi, hoi user

### Common AI Mistakes to Watch:
- AI viet stub/placeholder thay vi code that
- AI dung string concat thay vi library dung
- AI implement FE nhung bo sot BE
- AI khong biet data source nam o dau
- AI implement theo cach don gian nhat thay vi theo wireframe

## Validation Checklist
- [ ] Tat ca ISP steps da duoc audit
- [ ] Coverage matrix AP -> ISP day du
- [ ] Khong con step nao BLOCK
- [ ] Moi step co du 7 criteria

## Next Step
T310-001 — Build (implement tung step)

## References
- @docs/IncrementalStepPlan.md — load always (primary input)
- @docs/ArchitecturePack.md — load always (source of truth)
- @docs/Clevai_Tech_Stack_Prefer.md — load for tech stack verification
