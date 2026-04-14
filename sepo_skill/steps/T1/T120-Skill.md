---
name: sepo-t120
description: "T120 - L2: Cac Workflow"
---

# Skill: T120 — L2: Cac Workflow

**Expert Team:** Business Analyst + Process Designer

## Objective
Phan tich va mo ta cac workflow (luong nghiep vu) chinh cua he thong dua tren entities da xac dinh o L1.

**Scope:** Business Process Analysis, Workflow Design

## Trigger condition
Sau L1 (T110). Keywords: "workflow", "L2", "luong nghiep vu", "business process"

## Preconditions
- L1 (T110) da hoan thanh
- User da review L1

## Inputs
### Required
- File L1_VoiTong.md da tao o buoc truoc
- Tai lieu yeu cau (VOI)
### Optional
- Existing workflow docs tu version truoc
- User interview notes

## Output Contract
- File L2_Workflows.md theo format VoiTable
- Moi workflow: ten, actors, trigger, steps tong quat, output
- Workflow-to-Entity mapping

## Internal Reasoning Checklist
- [ ] He thong co nhung use case chinh nao?
- [ ] Moi use case ai la actor (user role)?
- [ ] Event nao trigger workflow?
- [ ] Workflow nao touch nhieu entity (cross-domain)?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `L1_VoiTong.md` | title chứa `VoiTong` | Nếu local chưa có → download |
| `L1 - L1_VoiTong` | title chứa `L1` | Nếu local chưa có → download |

**Procedure:**
```bash
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}
# 1. Login (nếu chưa có cookies)
curl -s -c cookies.txt -X POST \
  -H "Content-Type: application/json" \
  -d '{"email":"tuanpm@clevai.edu.vn"}' \
  ${BASE}/api/dev/quick-login

# 2. Lấy project ID (nếu chưa có)
PROJECT_ID=$(curl -s ${BASE}/api/projects -b cookies.txt | jq -r '.[0].id')

# 3. Lấy danh sách docs của project
curl -s "${BASE}/api/docs?projectId=${PROJECT_ID}" -b cookies.txt > docs_list.json

# 4. Với mỗi doc cần thiết:
#    - Filter: title chứa "VoiTong"
#    - Check local: ls **/*VoiTong* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "L1"
#    - Check local: ls **/*L1* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `L2_Workflows.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@L2_Workflows.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"L2_Workflows.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T120 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T120") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc L1 da tao
2. Xac dinh cac use cases/workflows chinh
3. Moi workflow: ten, actor, trigger, high-level steps, output
4. Map workflow → entities (table: workflow x entity)
5. Output: L2_Workflows.md
6. Self-review: moi entity trong L1 co duoc it nhat 1 workflow touch khong?

## Branching Logic
- Neu entity khong co workflow nao touch → co the la master data, OK
- Neu 1 workflow touch qua nhieu entities (>10) → xem xet tach nho

## Validation Checklist
- [ ] Moi workflow co actor + trigger + output
- [ ] Moi entity duoc touch boi it nhat 1 workflow (tru master data)
- [ ] Khong co workflow trung lap

## Failure Cases
- Thieu use case → chua cover het business requirements

## Recovery Actions
- Hoi user review va bo sung use cases con thieu

## Completion Criteria
- L2 file created
- Workflow-entity mapping complete
- User reviewed

## Next-Step Handoff
- T130 (L3: Chi tiet workflow steps)

## Reusable Prompt Block
```text
Ban la team chuyen gia gom Business Analyst + Process Designer.
Nhiem vu: Phan tich va mo ta cac workflow (luong nghiep vu) chinh cua he thong dua tren entities da xac dinh o L1.
Hay thuc hien theo Execution Procedure o tren.
```
