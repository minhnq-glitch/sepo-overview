---
name: sepo-t140
description: "T140 - Create Project Guide"
---

# Skill: T140 — Create Project Guide

**Expert Team:** Technical Writer + Project Manager

## Objective
Tao tai lieu huong dan du an cho developer moi join: setup, conventions, architecture overview.

**Scope:** Documentation, Onboarding

## Trigger condition
Sau L1-L3 hoan thanh. Keywords: "project guide", "huong dan du an"

## Preconditions
- L1, L2, L3 da hoan thanh

## Inputs
### Required
- L1, L2, L3 docs
- Codebase structure
### Optional
- Team conventions doc
- Code style guide

## Output Contract
- ProjectGuide.md: overview, setup, conventions, architecture, team roles

## Internal Reasoning Checklist
- [ ] Developer moi can biet gi de bat dau?
- [ ] Setup steps chinh xac?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `L1, L2, L3 docs` | title chứa `L1|L2|L3` | Nếu local chưa có → download |

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
#    - Filter: title chứa "L1|L2|L3"
#    - Check local: ls **/*L1|L2|L3* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `ProjectGuide.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@ProjectGuide.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"ProjectGuide.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T140 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T140") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc L1, L2, L3
2. Tong hop: project overview, tech stack, setup steps
3. Them: code conventions, branch strategy, PR process
4. Output: ProjectGuide.md

## Branching Logic
- Neu du an dung mono-repo → ghi ro cach navigate
- Neu multi-repo → list repos va muc dich

## Validation Checklist
- [ ] Guide du de developer moi setup ma khong can hoi them

## Failure Cases
- Guide thieu steps → developer mac ket khi setup

## Recovery Actions
- Thu setup theo guide tren may sach, fix gaps

## Completion Criteria
- ProjectGuide.md created
- Tested by following guide on clean machine

## Next-Step Handoff
- T2 (AP & ISP) — xay kien truc

## Reusable Prompt Block
```text
Ban la team chuyen gia gom Technical Writer + Project Manager.
Nhiem vu: Tao tai lieu huong dan du an cho developer moi join: setup, conventions, architecture overview.
Hay thuc hien theo Execution Procedure o tren.
```
