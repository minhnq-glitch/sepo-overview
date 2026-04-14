# CLAUDE.md — T220 — Tao Incremental Steps Plans cho BApp (L3: Step)

> **Level:** L3 — Downloaded when user starts step T220.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T220.

---

## Objective
Chia ArchitecturePack thanh cac buoc incremental (TDD approach) de developer implement tung step.

## Expert Team
Technical Program Lead + Software Architect + QA Lead

## Preconditions (MUST be true before starting)
- ArchitecturePack da hoan thanh va approved

## Output Contract (MUST deliver these)
- IncrementalStepPlan.md gom:
-   - Moi step: ID, scope, files affected, test criteria
-   - Dependencies giua cac steps
-   - Task classification (A/B/C) per step
-   - Estimated effort per step
-   - Build prompt + Test prompt cho moi step

## Validation Checklist
- [ ] Moi feature trong AP co it nhat 1 step
- [ ] Moi step co test criteria
- [ ] Dependencies khong circular
- [ ] Steps co the thuc hien tuan tu

## Failure Cases (watch out)
- Steps qua lon → kho implement
- Thieu step → feature khong duoc build

## Branching Logic
- Neu AP lon (>20 features) → nhom features thanh milestones
- Neu co dependencies complex → ve dependency graph

## SEPO Integration

### Before executing this step:
1. Login SEPO: `POST /api/dev/quick-login`
2. Check required docs exist locally (see skill file for list)
3. If missing: `GET /api/docs?projectId=X` → filter → download

### After completing this step:
1. Ask user: "Upload output to SEPO? (y/n)"
2. If yes: `POST /api/docs` → `POST /api/docs/{id}/versions`
3. Update task-item status: `PATCH /api/task-items/{id}` → doneStatus="Done"

## Completion Criteria (step DONE when)
- ISP file created
- All AP features covered
- User reviewed

## Next Step
- T230 (Audit ISP)

## Prompt Preview (see SKILL file for full prompt)
> You are a senior staff engineer and technical program lead.
> 
> I have already completed system design documentation in File [architecturepack_project]
> - **Phần 1:** EntityRelationshipDescription — Cấu trúc dữ liệu & quan hệ entity
> - **Phần 2:** ProcessDescription — Mô tả quy trình FEE / PENALTY / BONU...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
