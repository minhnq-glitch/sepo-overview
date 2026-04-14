# CLAUDE.md — T211 — Prep: Update DB Schema (L3: Step)

> **Level:** L3 — Downloaded when user starts step T211.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T211.

---

## Objective
Phan tich entities tu L1/AP va de xuat/update DB schema. Sinh migration SQL neu can.

## Expert Team
Database Architect + Backend Developer

## Preconditions (MUST be true before starting)
- L1 da co (hoac AP draft)
- Biet DB engine (PostgreSQL/MySQL)

## Output Contract (MUST deliver these)
- DB schema proposal: tables, columns, types, constraints
- Migration SQL file (neu can thay doi schema)
- ERD diagram (ASCII)

## Validation Checklist
- [ ] Moi entity trong L1 co table tuong ung
- [ ] Foreign keys dung
- [ ] Migration SQL chay duoc (dry-run)
- [ ] Khong mat data khi migrate

## Failure Cases (watch out)
- Migration lam mat data
- FK constraint violation

## Branching Logic
- Neu du an moi → tao toan bo schema tu dau
- Neu du an co → chi sinh ALTER statements cho thay doi

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
- Schema proposal reviewed
- Migration SQL ready

## Next Step
- T212 (Build ArchitecturePack)

## Prompt Preview (see SKILL file for full prompt)
> Hãy đề xuất ArchitecturePack  cho hệ thống này:
> 1. EntityRelationshipDescription
> 2. ProcessDescription
> 3. UIWireFrame
> 4. UserFlows
> 5. Complex Logic
> 6. Feature & Layer
> 7 Task classification
>     - Hãy đọc file [layerevent.md] để phân loại Task do user cung cấp theo tiêu chí sau:
>     - Loại A: Nếu chỉ ...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
