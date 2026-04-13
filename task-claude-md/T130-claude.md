# CLAUDE.md — Task T130: L3 Workflow Details

## Task Identity
- **Code:** T130
- **Name:** L3: Chi tiet tung buoc trong Workflow
- **Phase:** T1 — System Analysis
- **Classification:** Type A
- **Expert Team:** Business Analyst + UX Designer + Backend Developer

## Objective
Chi tiet hoa tung step trong moi workflow: data flow, UI interactions, API calls, validations.

## Preconditions
- L1 (T110) + L2 (T120) da hoan thanh

## Input Docs (Required)
- **L1_VoiTong.md** — Entity overview
- **L2_Workflows.md** — Workflow descriptions
- **Tai lieu yeu cau** — Requirements goc

## Output Contract
- **L3_WorkflowDetails.md**:
  - Moi step: input, output, actor, UI screen, API call, validation rules
  - Sequence diagrams (ASCII) cho cac flow chinh
  - CEP event mappings (neu co)

## Instructions

Ban la team chuyen gia gom Business Analyst + UX Designer + Backend Developer.

Nhiem vu: Chi tiet hoa tung step trong moi workflow: data flow, UI interactions, API calls, validations.

### Execution Steps:
1. Doc ky L1 va L2 de hieu entities va workflows
2. Voi moi step trong moi workflow, chi tiet:
   - **Input data**: du lieu dau vao tu dau?
   - **Output data**: du lieu dau ra la gi?
   - **Actor**: ai thuc hien?
   - **UI screen**: man hinh nao hien thi?
   - **API call**: endpoint nao duoc goi?
   - **Validation rules**: rang buoc gi can kiem tra?
3. Ve sequence diagrams (ASCII) cho cac flow chinh
4. Map CEP events (neu he thong co event-driven architecture)
5. Tao file L3_WorkflowDetails.md

## Validation Checklist
- [ ] Moi step co day du: input, output, actor, UI, API, validation
- [ ] Sequence diagrams cho cac flow chinh
- [ ] Khong co step mo ho (thieu input hoac output)
- [ ] API calls co endpoint cu the

## Next Step
T140 — Create Project Guide

## References
- @docs/L1_VoiTong.md — load always (entity reference)
- @docs/L2_Workflows.md — load always (workflow reference)
