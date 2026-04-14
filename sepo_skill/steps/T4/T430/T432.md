---
name: sepo-t432
description: "T432 - Execute B Tasks — Business Process"
---

# Skill: T432 — Execute B Tasks — Business Process

**Expert Team:** Backend Developer + Business Analyst

## Objective
Fix B-type feedback: business logic, API, data processing changes.

**Scope:** Backend, Business Logic, API

## Trigger condition
Sau T423, co B-type items. Keywords: "task B", "business logic fix"

## Preconditions
- Classified feedback (T423)
- B-type items exist

## Inputs
### Required
- B-type feedback items
- Codebase
- AP
### Optional

## Output Contract
- B-type items fixed
- Tests pass
- API behavior correct

## Internal Reasoning Checklist
- [ ] Schema change needed?
- [ ] API contract change?
- [ ] Backward compatible?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `Classified feedback (B-type items)` | title chứa `TestFeedback` | Nếu local chưa có → download |
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
#    - Filter: title chứa "TestFeedback"
#    - Check local: ls **/*TestFeedback* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "ArchitecturePack"
#    - Check local: ls **/*ArchitecturePack* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Doc B-type feedback items
2. Moi item: analyze impact, implement fix
3. Write tests for fix
4. Run all tests
5. Output: fixed items list

## Branching Logic
- Neu schema change → run migration (T411 again)
- Neu API change → update AP

## Validation Checklist
- [ ] Logic correct
- [ ] Tests pass
- [ ] No regressions

## Failure Cases
- Fix break other features

## Recovery Actions
- Revert, more careful fix with tests

## Completion Criteria
- All B items fixed
- Tests pass

## Next-Step Handoff
- T433 (C tasks) hoac T414 (re-deploy)

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
