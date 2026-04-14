# CLAUDE.md — T414 — Deploy len Staging (L3: Step)

> **Level:** L3 — Downloaded when user starts step T414.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T414.

---

## Objective
Deploy app len staging server de team test.

## Expert Team
DevOps Engineer + SRE

## Preconditions (MUST be true before starting)
- Code pushed (T413)
- Staging server accessible

## Output Contract (MUST deliver these)
- App deployed on staging
- Staging URL accessible
- Health check pass

## Validation Checklist
- [ ] Staging accessible
- [ ] Health check 200
- [ ] Basic flows work

## Failure Cases (watch out)
- Deploy fail
- Staging down after deploy

## Branching Logic
- Neu deploy fail → rollback to previous version

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
- Staging live
- Health pass

## Next Step
- T415 (Checkpoint STG)

## Prompt Preview (see SKILL file for full prompt)
> Hỏi người dùng đang thuộc "Usecase 1 :Nếu dự án xây mới lần đầu tiên" hay thuộc "Usecase 2: Nếu dự án này là sửa hệ thống đang có" thì sẽ sử dụng prompt tương ứng ở phía dưới đây:
> 
> Usecase 1: Nếu dự án xây mới lần đầu tiên thì sử dụng prompt
>  "Đọc file vault.md để lấy credential cho git,
> 1. Fix CI/C...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
