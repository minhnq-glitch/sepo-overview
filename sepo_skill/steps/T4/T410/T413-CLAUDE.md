# CLAUDE.md — T413 — Push len Git (L3: Step)

> **Level:** L3 — Downloaded when user starts step T413.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T413.

---

## Objective
Commit va push code len remote repository.

## Expert Team
DevOps Engineer + Git Specialist

## Preconditions (MUST be true before starting)
- SelfTest pass (T412)
- Git configured

## Output Contract (MUST deliver these)
- Code pushed to remote
- Commit message theo convention

## Validation Checklist
- [ ] Push thanh cong
- [ ] CI/CD triggered (neu co)
- [ ] No secrets in commit

## Failure Cases (watch out)
- Push rejected
- CI fail

## Branching Logic
- Neu conflict → resolve conflict, test lai, push
- Neu CI fail → fix, push lai

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
- Code on remote
- CI pass (neu co)

## Next Step
- T414 (Deploy Staging)

## Prompt Preview (see SKILL file for full prompt)
> Hỏi người dùng đang thuộc "Usecase 1 :Nếu dự án xây mới lần đầu tiên" hay thuộc "Usecase 2: Nếu dự án này là sửa hệ thống đang có" thì sẽ sử dụng prompt tương ứng ở phía dưới đây:
> Usecase 1: Nếu dự án xây mới lần đầu tiên thì sử dụng prompt
>  "Đọc file vault.md để lấy credential cho git, 
> 1. Dựa vào ...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
