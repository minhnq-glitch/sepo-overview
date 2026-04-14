---
name: sepo-t421
description: "T421 - Chuan bi data va testcase"
---

# Skill: T421 — Chuan bi data va testcase

**Expert Team:** QA Lead + Test Data Engineer

## Objective
Chuan bi test data va viet test cases tu AP specs cho UAT.

**Scope:** QA, Test Preparation

## Trigger condition
Truoc UAT. Keywords: "chuan bi test", "testcase", "data test"

## Preconditions
- Staging deployed
- AP da co

## Inputs
### Required
- ArchitecturePack.md
- Staging URL
### Optional
- TestFeedback template

## Output Contract
- Test cases document
- Test data ready on staging
- Test accounts created

## Internal Reasoning Checklist
- [ ] Test cases cover het AP features?
- [ ] Test data realistic?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `ArchitecturePack` | title chứa `ArchitecturePack` | Nếu local chưa có → download |

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
#    - Filter: title chứa "ArchitecturePack"
#    - Check local: ls **/*ArchitecturePack* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `TestCases.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@TestCases.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"TestCases.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T421 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T421") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc AP — list features can test
2. Viet test cases: moi feature >= 1 happy path + 1 error case
3. Tao test data tren staging (seed data, test accounts)
4. Output: TestCases.md + data ready

## Branching Logic
- Neu staging khong co data → seed data truoc

## Validation Checklist
- [ ] Moi feature co test case
- [ ] Test data ton tai tren staging

## Failure Cases
- Test data corrupt
- Staging down

## Recovery Actions
- Re-seed data
- Fix staging

## Completion Criteria
- Test cases written
- Data ready

## Next-Step Handoff
- T422 (UAT va feedback)

## Reusable Prompt Block
```text
Ban la team chuyen gia gom QA Lead + Test Data Engineer.
Nhiem vu: Chuan bi test data va viet test cases tu AP specs cho UAT.
Hay thuc hien theo Execution Procedure o tren.
```
