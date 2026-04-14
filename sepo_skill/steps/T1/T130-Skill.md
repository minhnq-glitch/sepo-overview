---
name: sepo-t130
description: "T130 - L3: Chi tiet tung buoc trong Workflow"
---

# Skill: T130 — L3: Chi tiet tung buoc trong Workflow

**Expert Team:** Business Analyst + UX Designer + Backend Developer

## Objective
Chi tiet hoa tung step trong moi workflow: data flow, UI interactions, API calls, validations.

**Scope:** Detailed Design, UI/UX, API, Data Flow

## Trigger condition
Sau L2 (T120). Keywords: "L3", "chi tiet workflow", "buoc trong workflow", "CEP"

## Preconditions
- L1 (T110) + L2 (T120) da hoan thanh

## Inputs
### Required
- L1_VoiTong.md
- L2_Workflows.md
- Tai lieu yeu cau
### Optional
- UI mockups
- API docs co san

## Output Contract
- File L3_WorkflowDetails.md
- Moi step: input, output, actor, UI screen, API call, validation rules
- Sequence diagrams (ASCII) cho cac flow chinh
- CEP event mappings (neu co)

## Internal Reasoning Checklist
- [ ] Moi step can nhung data gi (input)?
- [ ] Step tra ve gi (output)?
- [ ] Co validation rules nao?
- [ ] Step nao can goi API external?
- [ ] Step nao trigger event/notification?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `L1 docs` | title chứa `L1` | Nếu local chưa có → download |
| `L2 docs` | title chứa `L2` | Nếu local chưa có → download |

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
#    - Filter: title chứa "L1"
#    - Check local: ls **/*L1* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "L2"
#    - Check local: ls **/*L2* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `L3_WorkflowDetails.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@L3_WorkflowDetails.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"L3_WorkflowDetails.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T130 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T130") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc L1, L2
2. Voi moi workflow trong L2, chi tiet hoa tung step
3. Moi step: input data, output data, actor, UI screen, API call
4. Ve sequence diagram (ASCII) cho moi workflow
5. Xac dinh CEP events (neu co cross-system interaction)
6. Output: L3_WorkflowDetails.md
7. Self-review: moi step co du input/output/validation?

## Branching Logic
- Neu workflow don gian (CRUD) → mo ta ngan gon
- Neu workflow phuc tap (multi-step, multi-actor) → ve sequence diagram chi tiet

## Validation Checklist
- [ ] Moi step co input + output defined
- [ ] Sequence diagrams chinh xac
- [ ] API calls co method + path + request/response

## Failure Cases
- Step qua mo ho, khong du detail → developer khong hieu can lam gi

## Recovery Actions
- Hoi user clarify, hoac tham khao existing codebase

## Completion Criteria
- L3 file created
- All workflows detailed
- User reviewed

## Next-Step Handoff
- T140 (Project Guide) hoac T2 (AP & ISP)

## Reusable Prompt Block
```text
Ban la team chuyen gia gom Business Analyst + UX Designer + Backend Developer.
Nhiem vu: Chi tiet hoa tung step trong moi workflow: data flow, UI interactions, API calls, validations.
Hay thuc hien theo Execution Procedure o tren.
```
