---
name: sepo-t431
description: "T431 - Execute A Tasks — UI Customization"
---

# Skill: T431 — Execute A Tasks — UI Customization

**Expert Team:** Frontend Developer + UX Designer

## Objective
Fix A-type feedback: UI changes, styling, layout adjustments.

**Scope:** Frontend, UI/UX fixes

## Trigger condition
Sau T423, co A-type items. Keywords: "task A", "UI fix", "UI customization"

## Preconditions
- Classified feedback (T423)
- A-type items exist

## Inputs
### Required
- A-type feedback items
- Codebase
### Optional
- UI mockups
- Design system docs

## Output Contract
- A-type items fixed
- UI changes match feedback
- No regressions

## Internal Reasoning Checklist
- [ ] UI change chi anh huong frontend?
- [ ] Khong break existing functionality?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `Classified feedback (A-type items)` | title chứa `TestFeedback` | Nếu local chưa có → download |

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
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Doc A-type feedback items
2. Moi item: implement UI fix
3. Test: visual check + no regression
4. Output: fixed items list

## Branching Logic
- Neu UI fix can backend change → re-classify as B

## Validation Checklist
- [ ] UI matches feedback
- [ ] No regressions

## Failure Cases
- UI break khac page

## Recovery Actions
- Revert, fix carefully

## Completion Criteria
- All A items fixed

## Next-Step Handoff
- T432 (B tasks) hoac T414 (re-deploy)

## Reusable Prompt Block
```text
Task classification
    - Hãy đọc file [layerevent.md] để phân loại Task do user cung cấp theo tiêu chí sau:
    - Loại A: Nếu chỉ chỉnh sửa UI, API, không sửa Entity Schema
    -Loại B: Nếu có sửa Entity Schema, nhưng không đọc viết các bảng thuộc Layer Event và Event Details, và không viết các bảng thuộc Layer CalculateKR và ExtractEvent
    -Loại C: Nếu có sửa, đọc, viết các bảng thuộc Layer Event và Event Details, hoặc sửa, viết các bảng thuộc Layer CalculateKR và ExtractEvent

Lưu ý:
Nếu Task loại B, cần báo cho POSUP để duyệt Architecture Pack trước khi thực thi
Nếu Task loại C, cần báo cho POSUP và ARCH để duyệt Architecture Pack trước khi thực thi

```
