# CLAUDE.md — T120 — L2: Cac Workflow (L3: Step)

> **Level:** L3 — Downloaded when user starts step T120.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T120.

---

## Objective
Phan tich va mo ta cac workflow (luong nghiep vu) chinh cua he thong dua tren entities da xac dinh o L1.

## Expert Team
Business Analyst + Process Designer

## Preconditions (MUST be true before starting)
- L1 (T110) da hoan thanh
- User da review L1

## Output Contract (MUST deliver these)
- File L2_Workflows.md theo format VoiTable
- Moi workflow: ten, actors, trigger, steps tong quat, output
- Workflow-to-Entity mapping

## Validation Checklist
- [ ] Moi workflow co actor + trigger + output
- [ ] Moi entity duoc touch boi it nhat 1 workflow (tru master data)
- [ ] Khong co workflow trung lap

## Failure Cases (watch out)
- Thieu use case → chua cover het business requirements

## Branching Logic
- Neu entity khong co workflow nao touch → co the la master data, OK
- Neu 1 workflow touch qua nhieu entities (>10) → xem xet tach nho

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
- L2 file created
- Workflow-entity mapping complete
- User reviewed

## Next Step
- T130 (L3: Chi tiet workflow steps)

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
