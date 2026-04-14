# CLAUDE.md — T510 — Deploy Production (L3: Step)

> **Level:** L3 — Downloaded when user starts step T510.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T510.

---

## Objective
Deploy app len production environment.

## Expert Team
DevOps Engineer + SRE + Database Architect

## Preconditions (MUST be true before starting)
- UAT pass
- All critical bugs fixed
- Staging verified

## Output Contract (MUST deliver these)
- App live on production
- Health check pass
- No errors in monitoring

## Validation Checklist
- [ ] Health check 200
- [ ] No errors
- [ ] Key flows work

## Failure Cases (watch out)
- Deploy fail
- Migration fail
- Errors after deploy

## Branching Logic
- Neu errors after deploy → rollback
- Neu migration fail → rollback DB from backup

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
- Production live
- No errors for 30 min

## Next Step
- T530 (Checkpoint PRD)

## Prompt Preview (see SKILL file for full prompt)
> - Nếu database có change thì hãy tổng hợp tất cả các change liên quan đến database vào 1 file changedb.sql
> - Nếu có bổ sung thêm key hoặc token... thì hãy tổng hợp tất cả các key đó vào file changevault.md
> - Tạo merge request từ nhánh code hiện tại vào nhánh prod-c và kèm file changedb.sql ở trên và...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
