# CLAUDE.md — T380 — Hoan thanh Incremental Step Plan (L3: Step)

> **Level:** L3 — Downloaded when user starts step T380.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T380.

---

## Objective
Dong goi ket qua build: update ISP status, commit code, chuan bi cho deployment.

## Expert Team
DevOps Engineer + Technical Writer

## Preconditions (MUST be true before starting)
- Tat ca ISP steps da build (T310) va test (T320)

## Output Contract (MUST deliver these)
- ISP updated: all steps marked complete
- Code committed va pushed
- Ready for deploy

## Validation Checklist
- [ ] All ISP steps done
- [ ] Git pushed
- [ ] No uncommitted changes

## Failure Cases (watch out)
- Push fail → credentials sai

## Branching Logic
- Neu co steps chua done → list ra va hoi user

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
- ISP 100% done
- Code pushed

## Next Step
- T390 (Eval BApp voi AP)

## Prompt Preview (see SKILL file for full prompt)
> Đọc file vault.md để lấy credential cho git, 
> 1. Dựa vào quy luật đặt mã và tên hệ thống trong file GIT-DB-Structure.md.
> - Từ đầu tiên là đối tượng người dùng chính (VD: Student, Teacher, Parent, Sale, ...)
> - Từ thứ 2 là chức năng chính hệ thống (VD: Tutoring, Education, Report, Compensation, ....)
> ...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
