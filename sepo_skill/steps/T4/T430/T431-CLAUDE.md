# CLAUDE.md — T431 — Execute A Tasks — UI Customization (L3: Step)

> **Level:** L3 — Downloaded when user starts step T431.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T431.

---

## Objective
Fix A-type feedback: UI changes, styling, layout adjustments.

## Expert Team
Frontend Developer + UX Designer

## Preconditions (MUST be true before starting)
- Classified feedback (T423)
- A-type items exist

## Output Contract (MUST deliver these)
- A-type items fixed
- UI changes match feedback
- No regressions

## Validation Checklist
- [ ] UI matches feedback
- [ ] No regressions

## Failure Cases (watch out)
- UI break khac page

## Branching Logic
- Neu UI fix can backend change → re-classify as B

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
- All A items fixed

## Next Step
- T432 (B tasks) hoac T414 (re-deploy)

## Prompt Preview (see SKILL file for full prompt)
> Task classification
>     - Hãy đọc file [layerevent.md] để phân loại Task do user cung cấp theo tiêu chí sau:
>     - Loại A: Nếu chỉ chỉnh sửa UI, API, không sửa Entity Schema
>     -Loại B: Nếu có sửa Entity Schema, nhưng không đọc viết các bảng thuộc Layer Event và Event Details, và không viết các bản...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
