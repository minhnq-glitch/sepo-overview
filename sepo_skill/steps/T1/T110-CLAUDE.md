# CLAUDE.md — T110 — L1: Voi tong — Entity Overview (L3: Step)

> **Level:** L3 — Downloaded when user starts step T110.
> **Download:** Into project folder, overrides project CLAUDE.md.
> **Activate:** Injected into all prompts while executing step T110.

---

## Objective
Phan tich he thong va tao document L1 (Voi tong): tong quan cac entities, relationships, va system boundary.

## Expert Team
Business Analyst + System Architect

## Preconditions (MUST be true before starting)
- Du an da setup (T010 done)
- Co tai lieu yeu cau (VOI, feature doc, hoac existing codebase)
- Cac file input da duoc convert sang markdown (neu la Gsheet/Excel)

## Output Contract (MUST deliver these)
- File L1_VoiTong.md theo format VoiTable
- Liet ke TAT CA entities voi: ten, mo ta, relationships
- Entity layer classification (A: Reference, B: Plan, C: Transaction)
- System boundary diagram (ASCII)

## Validation Checklist
- [ ] Moi entity co ten + mo ta + layer classification
- [ ] Moi relationship co cardinality (1-1, 1-N, N-N)
- [ ] System boundary diagram hien thi dung
- [ ] Khong co entity mo co (orphan entity khong relationship)
- [ ] Entity names nhat quan (khong duplicate, khong typo)

## Failure Cases (watch out)
- Tai lieu input khong du → khong the xac dinh het entities
- Entity relationships khong ro → mo ho ve cardinality
- Domain qua lon → L1 qua dai, kho doc

## Branching Logic
- Neu he thong moi hoan toan → tao L1 tu dau dua tren VOI
- Neu he thong da co → scan codebase (DB schema, models) de extract entities
- Neu co nhieu module → tao L1 cho tung module, roi merge

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
- File L1_VoiTong.md da tao
- TAT CA entities da liet ke voi layer classification
- Relationships da ve
- User da review va confirm

## Next Step
- T120 (L2: Cac Workflow) — chi tiet hoa cac luong nghiep vu

## Prompt Preview (see SKILL file for full prompt)
> Chuyển các File dưới đây thành file dạng MarkDown...

## Global Rules
Read `sepo_skill/CLAUDE.md` (L1) for Iron Rules, Git conventions, Tech Stack, SEPO API reference.
