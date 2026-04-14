---
name: sepo-t510
description: "T510 - Deploy Production"
---

# Skill: T510 — Deploy Production

**Expert Team:** DevOps Engineer + SRE + Database Architect

## Objective
Deploy app len production environment.

**Scope:** Production Deployment

## Trigger condition
Sau UAT pass. Keywords: "deploy production", "deploy prod"

## Preconditions
- UAT pass
- All critical bugs fixed
- Staging verified

## Inputs
### Required
- Production credentials
- Migration scripts (neu co)
- Deploy process
### Optional
- Rollback plan

## Output Contract
- App live on production
- Health check pass
- No errors in monitoring

## Internal Reasoning Checklist
- [ ] Migration needed?
- [ ] Backup done?
- [ ] Rollback plan ready?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `migration scripts` | title chứa `migration` | Nếu local chưa có → download |

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
#    - Filter: title chứa "migration"
#    - Check local: ls **/*migration* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Neu DB changes → tong hop migration scripts
2. Backup production DB
3. Run migration (neu co)
4. Deploy code
5. Verify health check
6. Monitor errors for 30 minutes

## Branching Logic
- Neu errors after deploy → rollback
- Neu migration fail → rollback DB from backup

## Validation Checklist
- [ ] Health check 200
- [ ] No errors
- [ ] Key flows work

## Failure Cases
- Deploy fail
- Migration fail
- Errors after deploy

## Recovery Actions
- Rollback code + DB
- Investigate, fix, re-deploy

## Completion Criteria
- Production live
- No errors for 30 min

## Next-Step Handoff
- T530 (Checkpoint PRD)

## Reusable Prompt Block
```text
- Nếu database có change thì hãy tổng hợp tất cả các change liên quan đến database vào 1 file changedb.sql
- Nếu có bổ sung thêm key hoặc token... thì hãy tổng hợp tất cả các key đó vào file changevault.md
- Tạo merge request từ nhánh code hiện tại vào nhánh prod-c và kèm file changedb.sql ở trên và changevault.md nếu có.



```
