# CLAUDE.md — Task T110: L1 Voi Tong (Entity Overview)

## Task Identity
- **Code:** T110
- **Name:** L1: Voi tong — Entity Overview
- **Phase:** T1 — System Analysis
- **Classification:** Type A
- **Expert Team:** Business Analyst + System Architect

## Objective
Phan tich he thong va tao document L1 (Voi tong): tong quan cac entities, relationships, va system boundary.

## Preconditions
- T010 (Setup) da hoan thanh
- Co tai lieu yeu cau (VOI, feature doc, hoac existing codebase)
- Cac file input da duoc convert sang markdown (neu la Gsheet/Excel)

## Input Docs (Required)
- **VOI / Requirement docs** — Tai lieu mo ta he thong
- **Existing codebase** (neu co) — De phan tich entities hien tai

## Output Contract
- **L1_VoiTong.md** theo format VoiTable:
  - Liet ke TAT CA entities voi: ten, mo ta, relationships
  - Entity layer classification (A: Reference, B: Plan, C: Transaction)
  - System boundary diagram (ASCII)

## Instructions

Ban la team chuyen gia gom Business Analyst + System Architect.

Nhiem vu: Chuyen cac file input thanh file dang MarkDown theo format VoiTable.

### Execution Steps:
1. Doc tat ca tai lieu input (VOI, requirement docs, codebase)
2. Xac dinh tat ca entities trong he thong
3. Phan loai moi entity theo layer:
   - **Layer A (Reference):** Du lieu tham chieu, it thay doi (VD: Country, Currency)
   - **Layer B (Plan):** Du lieu ke hoach, thay doi theo chu ky (VD: Schedule, Budget)
   - **Layer C (Transaction):** Du lieu giao dich, thay doi thuong xuyen (VD: Order, Payment)
4. Mo ta relationships giua cac entities
5. Ve system boundary diagram (ASCII)
6. Tao file L1_VoiTong.md theo format VoiTable

### VoiTable Format:
| Entity | Description | Layer | Relationships |
|--------|------------|-------|---------------|
| ... | ... | A/B/C | ... |

## Validation Checklist
- [ ] Tat ca entities da duoc liet ke
- [ ] Moi entity co ten, mo ta, layer classification
- [ ] Relationships giua entities da duoc mo ta
- [ ] System boundary diagram da ve
- [ ] File output dung format VoiTable

## Next Step
T120 — L2: Cac Workflow

## References
- @docs/layerevent.md — load when classifying entity layers
- @skills/sepo_skill/kb/sepo_architecture.md — load for architecture patterns
