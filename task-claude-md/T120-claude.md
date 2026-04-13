# CLAUDE.md — Task T120: L2 Workflows

## Task Identity
- **Code:** T120
- **Name:** L2: Cac Workflow
- **Phase:** T1 — System Analysis
- **Classification:** Type A
- **Expert Team:** Business Analyst + Process Designer

## Objective
Phan tich va mo ta cac workflow (luong nghiep vu) chinh cua he thong dua tren entities da xac dinh o L1.

## Preconditions
- L1 (T110) da hoan thanh
- User da review L1

## Input Docs (Required)
- **L1_VoiTong.md** — Entity overview tu buoc truoc
- **VOI / Requirement docs** — Tai lieu yeu cau goc

## Output Contract
- **L2_Workflows.md** theo format VoiTable:
  - Moi workflow: ten, actors, trigger, steps tong quat, output
  - Workflow-to-Entity mapping
  - Use case diagram (ASCII)

## Instructions

Ban la team chuyen gia gom Business Analyst + Process Designer.

Nhiem vu: Phan tich va mo ta cac workflow (luong nghiep vu) chinh cua he thong dua tren entities da xac dinh o L1.

### Execution Steps:
1. Doc ky L1_VoiTong.md de hieu cac entities
2. Xac dinh tat ca workflows chinh trong he thong
3. Voi moi workflow, mo ta:
   - **Ten workflow**
   - **Actors** (ai tham gia)
   - **Trigger** (dieu kien khoi dong)
   - **Steps tong quat** (cac buoc chinh)
   - **Output** (ket qua cuoi cung)
4. Tao Workflow-to-Entity mapping: moi workflow tac dong den entities nao
5. Ve use case diagram (ASCII)
6. Tao file L2_Workflows.md

## Validation Checklist
- [ ] Tat ca workflows chinh da duoc liet ke
- [ ] Moi workflow co day du: actors, trigger, steps, output
- [ ] Workflow-to-Entity mapping day du
- [ ] Khong co workflow orphan (khong lien ket entity nao)

## Next Step
T130 — L3: Chi tiet tung buoc trong Workflow

## References
- @docs/L1_VoiTong.md — load always (primary input)
