---
name: sepo-t423
description: "T423 - Classify feedback (A/B/C)"
---

# Skill: T423 — Classify feedback (A/B/C)

**Expert Team:** QA Lead + Product Manager + Software Architect

## Objective
Phan loai moi feedback item theo A (UI), B (Business), C (Performance) de prioritize fixes.

**Scope:** Feedback triage, Classification

## Trigger condition
Sau UAT (T422). Keywords: "classify", "phan loai feedback", "A/B/C"

## Preconditions
- TestFeedback file da co (T422)

## Inputs
### Required
- TestFeedback file
- layerevent.md (classification rules)
### Optional

## Output Contract
- Classified feedback: moi item co A/B/C label + priority

## Internal Reasoning Checklist
- [ ] Moi item da classified?
- [ ] Classification dung theo layerevent.md?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `TestFeedback file` | title chứa `TestFeedback` | Nếu local chưa có → download |
| `layerevent.md` | title chứa `layerevent` | Nếu local chưa có → download |

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
#    - Filter: title chứa "TestFeedback"
#    - Check local: ls **/*TestFeedback* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "layerevent"
#    - Check local: ls **/*layerevent* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `TestFeedback_Classified_{date}.xlsx`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@TestFeedback_Classified_{date}.xlsx" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"TestFeedback_Classified_{date}.xlsx","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T423 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T423") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc TestFeedback file
2. Doc layerevent.md de hieu classification rules
3. Moi item: A (UI only) / B (Business logic) / C (Performance)
4. Assign priority: Critical > High > Medium > Low
5. Output: classified feedback list

## Branching Logic
- Neu nhieu Critical → bao PO de re-prioritize

## Validation Checklist
- [ ] ALL items classified
- [ ] Classification consistent

## Failure Cases
- Ambiguous items

## Recovery Actions
- Discuss voi team de classify

## Completion Criteria
- All items classified
- Priorities assigned

## Next-Step Handoff
- T431/T432/T433 (Execute A/B/C tasks)

## Reusable Prompt Block
```text
1. Đọc kĩ file [T720_TestFeedback_Template] và mô tả lại hiểu biết của bạn về các bug và feature đó
2. Đưa ra giải pháp
3. Nếu các bug và feature chưa rõ ràng thì hãy hỏi lại, không được suy diễn và tự bịa giải pháp
4. Sử dụng claude.md

```
