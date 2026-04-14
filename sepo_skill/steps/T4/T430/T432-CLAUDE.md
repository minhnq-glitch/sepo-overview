# CLAUDE.md — T432 — Execute B Tasks — Business Process (L3: Step)

> **Level:** L3 — Downloaded when user starts step T432.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T432.

---

## Objective
Fix B-type feedback: business logic, API, data processing changes.

## Expert Team
Backend Developer + Business Analyst

## Preconditions (MUST be true before starting)
- Classified feedback (T423)
- B-type items exist

## Output Contract (MUST deliver these)
- B-type items fixed
- Tests pass
- API behavior correct

## Validation Checklist
- [ ] Logic correct
- [ ] Tests pass
- [ ] No regressions

## Failure Cases (watch out)
- Fix break other features

## Branching Logic
- Neu schema change → run migration (T411 again)
- Neu API change → update AP

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
- All B items fixed
- Tests pass

## Next Step
- T433 (C tasks) hoac T414 (re-deploy)

## Prompt Preview (see SKILL file for full prompt)
> Task classification
>     - Hãy đọc file [layerevent.md] để phân loại Task do user cung cấp theo tiêu chí sau:
>     - Loại A: Nếu chỉ chỉnh sửa UI, API, không sửa Entity Schema
>     -Loại B: Nếu có sửa Entity Schema, nhưng không đọc viết các bảng thuộc Layer Event và Event Details, và không viết các bản...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
