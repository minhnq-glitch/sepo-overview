# CLAUDE.md — Task T140: Project Guide

## Task Identity
- **Code:** T140
- **Name:** Create Project Guide
- **Phase:** T1 — System Analysis
- **Classification:** Type A
- **Expert Team:** Technical Writer + Project Manager

## Objective
Tao tai lieu huong dan du an cho developer moi join: setup, conventions, architecture overview.

## Preconditions
- L1 (T110), L2 (T120), L3 (T130) da hoan thanh

## Input Docs (Required)
- **L1_VoiTong.md** — Entity overview
- **L2_Workflows.md** — Workflow descriptions
- **L3_WorkflowDetails.md** — Detailed workflow steps
- **Codebase structure** — Hien trang code

## Output Contract
- **ProjectGuide.md** bao gom:
  - Project overview va muc tieu
  - Setup instructions (dev environment)
  - Code conventions va standards
  - Architecture overview (high-level)
  - Team roles va responsibilities
  - Key workflows summary

## Instructions

Ban la team chuyen gia gom Technical Writer + Project Manager.

Nhiem vu: Tao tai lieu huong dan du an cho developer moi join.

### Execution Steps:
1. Doc L1, L2, L3 de hieu toan canh du an
2. Tong hop project overview: muc tieu, scope, tech stack
3. Viet setup instructions chi tiet (tu T010)
4. Mo ta code conventions: naming, folder structure, patterns
5. Tao architecture overview diagram (high-level)
6. Liet ke team roles va responsibilities
7. Tao key workflows summary (tu L2)
8. Tao file ProjectGuide.md

## Validation Checklist
- [ ] Developer moi co the setup tu ProjectGuide ma khong can hoi
- [ ] Conventions ro rang va nhat quan
- [ ] Architecture overview dung voi thuc te code
- [ ] Tat ca workflows chinh da duoc tom tat

## Next Step
T211 — Prep: Update DB Schema (Phase T2)

## References
- @docs/L1_VoiTong.md — load for entity context
- @docs/L2_Workflows.md — load for workflow context
- @docs/L3_WorkflowDetails.md — load for detail context
