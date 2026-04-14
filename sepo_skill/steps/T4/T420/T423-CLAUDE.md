# CLAUDE.md — T423 — Classify feedback (A/B/C) (L3: Step)

> **Level:** L3 — Downloaded when user starts step T423.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T423.

---

## Objective
Phan loai moi feedback item theo A (UI), B (Business), C (Performance) de prioritize fixes.

## Expert Team
QA Lead + Product Manager + Software Architect

## Preconditions (MUST be true before starting)
- TestFeedback file da co (T422)

## Output Contract (MUST deliver these)
- Classified feedback: moi item co A/B/C label + priority

## Validation Checklist
- [ ] ALL items classified
- [ ] Classification consistent

## Failure Cases (watch out)
- Ambiguous items

## Branching Logic
- Neu nhieu Critical → bao PO de re-prioritize

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
- All items classified
- Priorities assigned

## Next Step
- T431/T432/T433 (Execute A/B/C tasks)

## Prompt Preview (see SKILL file for full prompt)
> 1. Đọc kĩ file [T720_TestFeedback_Template] và mô tả lại hiểu biết của bạn về các bug và feature đó
> 2. Đưa ra giải pháp
> 3. Nếu các bug và feature chưa rõ ràng thì hãy hỏi lại, không được suy diễn và tự bịa giải pháp
> 4. Sử dụng claude.md
> ...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
