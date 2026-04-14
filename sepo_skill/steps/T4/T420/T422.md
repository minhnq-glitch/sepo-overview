---
name: sepo-t422
description: "T422 - UAT va feature feedback"
---

# Skill: T422 — UAT va feature feedback

**Expert Team:** QA Tester + UX Reviewer

## Objective
Thuc hien User Acceptance Testing, ghi nhan bugs va feedback.

**Scope:** UAT, Bug reporting

## Trigger condition
Sau T421. Keywords: "UAT", "test feedback"

## Preconditions
- Test cases ready (T421)
- Staging deployed

## Inputs
### Required
- Test cases
- Staging URL
- T720_TestFeedback_Template
### Optional

## Output Contract
- TestFeedback file: moi bug/feedback gom mo ta, steps to reproduce, severity, evidence

## Internal Reasoning Checklist
- [ ] Moi test case da execute?
- [ ] Bugs co steps to reproduce?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `T720_TestFeedback_Template` | title chứa `T720` | Nếu local chưa có → download |
| `TestFeedback_Template` | title chứa `TestFeedback_Template` | Nếu local chưa có → download |

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
#    - Filter: title chứa "T720"
#    - Check local: ls **/*T720* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "TestFeedback_Template"
#    - Check local: ls **/*TestFeedback_Template* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `TestFeedback_{date}.xlsx`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@TestFeedback_{date}.xlsx" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"TestFeedback_{date}.xlsx","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T422 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T422") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Download T720_TestFeedback_Template
2. Execute moi test case tren staging
3. Ghi nhan: pass/fail, bugs, feedback
4. Moi bug: mo ta, steps to reproduce, expected vs actual, screenshot
5. Output: TestFeedback_{date}.xlsx

## Branching Logic
- Neu critical bug → block release, bao team ngay

## Validation Checklist
- [ ] ALL test cases executed
- [ ] Moi bug co steps to reproduce

## Failure Cases
- Staging down during UAT

## Recovery Actions
- Wait for staging fix, continue UAT

## Completion Criteria
- All test cases executed
- Feedback file complete

## Next-Step Handoff
- T423 (Classify feedback)

## Reusable Prompt Block
```text
Download và sử dụng file T720_TestFeedback_Template để điền thông tin UAT
```
