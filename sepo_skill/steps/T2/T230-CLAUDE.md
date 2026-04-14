# CLAUDE.md — T230 — Audit Incremental Steps Plans (L3: Step)

> **Level:** L3 — Downloaded when user starts step T230.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T230.

---

## Objective
Kiem tra chat luong ISP: coverage, clarity, test criteria, va enrich voi data/API/UI details.

## Expert Team
ISP Quality Auditor + Software Architect

## Preconditions (MUST be true before starting)
- ISP (T220) da tao
- AP da approved

## Output Contract (MUST deliver these)
- IncrementalStepPlanAudited.md:
-   - Moi step da enriched voi chi tiet
-   - Coverage matrix: AP section → ISP step
-   - Gaps identified va resolved
-   - Status: ENRICH (ok) hoac BLOCK (can fix)

## Validation Checklist
- [ ] Coverage >= 95% (AP → ISP)
- [ ] Khong co BLOCK step nao con lai
- [ ] Moi step co test criteria cu the

## Failure Cases (watch out)
- Coverage thap → se co features khong duoc build

## Branching Logic
- Neu gap nhieu BLOCK steps → bao user truoc khi tiep tuc
- Neu coverage < 90% → can them steps

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
- Audited ISP created
- 95%+ coverage
- No BLOCK steps remaining

## Next Step
- T3 (Build BApp) — bat dau code

## Prompt Preview (see SKILL file for full prompt)
> # Vai trò
> Bạn là ISP Quality Auditor. Nhiệm vụ: kiểm tra từng ISP step có đủ detail
> để một AI coding agent (Claude Code) implement chính xác hay không.
> 
> Bạn có kinh nghiệm thực tế với các lỗi thường gặp khi ISP thiếu detail:
> - AI viết stub/placeholder thay vì code thật
> - AI dùng string concat thay v...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
