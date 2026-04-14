---
name: sepo-t415
description: "T415 - Create Checkpoint AP-Code-DB STG"
---

# Skill: T415 — Create Checkpoint AP-Code-DB STG

**Expert Team:** Technical Writer + DevOps Engineer

## Objective
Luu checkpoint: version commits, DB schema snapshot, docs cho staging version.

**Scope:** Documentation, Version tracking

## Trigger condition
Sau deploy staging (T414). Keywords: "checkpoint STG"

## Preconditions
- Staging deployed va verified

## Inputs
### Required
- Git log
- DB schema
- AP version
### Optional

## Output Contract
- STG_VersionCommit.md
- STG_DB_Schema.md
- Checkpoint doc

## Internal Reasoning Checklist
- [ ] Git log day du?
- [ ] DB schema exported?

## 📥 Download từ SEPO (trước khi execute)

> Step này **không cần download docs** từ SEPO.

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `STG_VersionCommit_{version}.md`
- 📄 `STG_DB_Schema_{version}.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@STG_VersionCommit_{version}.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"STG_VersionCommit_{version}.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T415 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T415") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Export git log: git log --oneline > STG_VersionCommit.md
2. Export DB schema snapshot
3. Luu checkpoint docs vao docs/
4. Verify: files created

## Branching Logic

## Validation Checklist
- [ ] Checkpoint files exist
- [ ] Git log accurate
- [ ] Schema matches

## Failure Cases
- Export fail

## Recovery Actions
- Manual export

## Completion Criteria
- Checkpoint files saved

## Next-Step Handoff
- T420 (UAT Staging)

## Reusable Prompt Block
```text
Tổng hợp tất cả các commit git/repo vào file STG_VersionCommit.md với tên thêm hậu tố {version}_{date} (v3_190326)
Export ra DB schema ra file : STG_DB_Schema_{version}_{date}
```
