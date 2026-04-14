# CLAUDE.md — T130 — L3: Chi tiet tung buoc trong Workflow (L3: Step)

> **Level:** L3 — Downloaded when user starts step T130.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T130.

---

## Objective
Chi tiet hoa tung step trong moi workflow: data flow, UI interactions, API calls, validations.

## Expert Team
Business Analyst + UX Designer + Backend Developer

## Preconditions (MUST be true before starting)
- L1 (T110) + L2 (T120) da hoan thanh

## Output Contract (MUST deliver these)
- File L3_WorkflowDetails.md
- Moi step: input, output, actor, UI screen, API call, validation rules
- Sequence diagrams (ASCII) cho cac flow chinh
- CEP event mappings (neu co)

## Validation Checklist
- [ ] Moi step co input + output defined
- [ ] Sequence diagrams chinh xac
- [ ] API calls co method + path + request/response

## Failure Cases (watch out)
- Step qua mo ho, khong du detail → developer khong hieu can lam gi

## Branching Logic
- Neu workflow don gian (CRUD) → mo ta ngan gon
- Neu workflow phuc tap (multi-step, multi-actor) → ve sequence diagram chi tiet

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
- L3 file created
- All workflows detailed
- User reviewed

## Next Step
- T140 (Project Guide) hoac T2 (AP & ISP)

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
