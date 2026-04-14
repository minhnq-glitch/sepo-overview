---
name: sepo-t530
description: "T530 - Create Checkpoint AP-Code-DB PRD"
---

# Skill: T530 — Create Checkpoint AP-Code-DB PRD

**Expert Team:** Technical Writer + DevOps Engineer

## Objective
Luu checkpoint production: version commits, DB schema, docs.

**Scope:** Documentation, Production version tracking

## Trigger condition
Sau deploy production (T510). Keywords: "checkpoint PRD"

## Preconditions
- Production deployed va stable

## Inputs
### Required
- Git log
- Production DB schema
- AP version
### Optional

## Output Contract
- PRD_VersionCommit.md
- PRD_DB_Schema.md

## Internal Reasoning Checklist
- [ ] Git log exported?
- [ ] Schema snapshot taken?

## 📥 Download từ SEPO (trước khi execute)

> Step này **không cần download docs** từ SEPO.

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `PRD_VersionCommit_{version}.md`
- 📄 `PRD_DB_Schema_{version}.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@PRD_VersionCommit_{version}.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"PRD_VersionCommit_{version}.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T530 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T530") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Export git log production
2. Export production DB schema
3. Save checkpoint docs
4. Verify files

## Branching Logic

## Validation Checklist
- [ ] Files exist and accurate

## Failure Cases
- Export fail

## Recovery Actions
- Manual export

## Completion Criteria
- Checkpoint files saved

## Next-Step Handoff
- Done! Hoac quay lai T1 cho version tiep theo.

## Reusable Prompt Block
```text
Tổng hợp tất cả các commit git/repo vào file PRD_VersionCommit.md với tên thêm hậu tố vxx.date (v3_190326)
Export ra DB schema ra file : PRD_DB_Schema_{version}_{date}
```
