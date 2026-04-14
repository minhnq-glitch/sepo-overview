---
name: sepo-t380
description: "T380 - Hoan thanh Incremental Step Plan"
---

# Skill: T380 — Hoan thanh Incremental Step Plan

**Expert Team:** DevOps Engineer + Technical Writer

## Objective
Dong goi ket qua build: update ISP status, commit code, chuan bi cho deployment.

**Scope:** Release management, Git, Documentation

## Trigger condition
Sau khi tat ca ISP steps da build + test. Keywords: "hoan thanh ISP", "complete plan"

## Preconditions
- Tat ca ISP steps da build (T310) va test (T320)

## Inputs
### Required
- ISP file
- Git repo
- vault.md (credentials)
### Optional

## Output Contract
- ISP updated: all steps marked complete
- Code committed va pushed
- Ready for deploy

## Internal Reasoning Checklist
- [ ] Tat ca steps done?
- [ ] Tests pass?
- [ ] Git clean?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `vault.md (credentials)` | title chứa `vault` | Nếu local chưa có → download |

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
#    - Filter: title chứa "vault"
#    - Check local: ls **/*vault* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Doc vault.md de lay git credentials
2. Review ISP: mark all completed steps
3. Git add → commit → push
4. Verify: git status clean
5. Output: updated ISP + git log

## Branching Logic
- Neu co steps chua done → list ra va hoi user

## Validation Checklist
- [ ] All ISP steps done
- [ ] Git pushed
- [ ] No uncommitted changes

## Failure Cases
- Push fail → credentials sai

## Recovery Actions
- Check vault.md, verify remote access

## Completion Criteria
- ISP 100% done
- Code pushed

## Next-Step Handoff
- T390 (Eval BApp voi AP)

## Reusable Prompt Block
```text
Đọc file vault.md để lấy credential cho git, 
1. Dựa vào quy luật đặt mã và tên hệ thống trong file GIT-DB-Structure.md.
- Từ đầu tiên là đối tượng người dùng chính (VD: Student, Teacher, Parent, Sale, ...)
- Từ thứ 2 là chức năng chính hệ thống (VD: Tutoring, Education, Report, Compensation, ....)
- Từ thứ 3 là giao diện chính của hệ thống (VD: Web, App, Portal, ...)
Mã được ghép từ những chữ cái đầu của các tên.
Hãy gợi ý 3 mã và tên cho để tôi chọn
2. Merge code backend và frontend vào chung 1 repo với name lấy từ bước 1
3.  push code trong repo chung này lên git này https://gitlab.clevai.vn/fe/[repo name]
4. Tạo Merge Request lên staging-c
5. Fix CI/CD build fail
6. Tự xử lý Vault
7. Check code local và push
8. chuyển sang nhánh staging-c để làm việc
9. Tạo domain theo tên đã chọn với format : [repo name].mikai.tech
```
