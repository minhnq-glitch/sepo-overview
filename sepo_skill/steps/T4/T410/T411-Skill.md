---
name: sepo-t411
description: "T411 - Run Migration"
---

# Skill: T411 — Run Migration

**Expert Team:** Database Architect + DevOps Engineer

## Objective
Chay database migration scripts tren target environment.

**Scope:** Database, Migration, DevOps

## Trigger condition
Dau phase deploy. Keywords: "migration", "run migration"

## Preconditions
- Migration SQL da co (tu T211)
- Target DB accessible

## Inputs
### Required
- Migration SQL files
- DB connection credentials
### Optional
- Rollback scripts

## Output Contract
- Migration executed successfully
- Schema verified
- No data loss

## Internal Reasoning Checklist
- [ ] Migration reversible?
- [ ] Backup truoc khi chay?
- [ ] Data migration needed?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `migration.sql` | title chứa `migration` | Nếu local chưa có → download |
| `vault.md` | title chứa `vault` | Nếu local chưa có → download |

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
#    - Filter: title chứa "vault"
#    - Check local: ls **/*vault* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Backup current DB (pg_dump / mysqldump)
2. Review migration SQL 1 lan nua
3. Chay migration (dry-run truoc neu co the)
4. Verify schema sau migration
5. Test: critical queries van chay dung

## Branching Logic
- Neu migration fail → rollback tu backup
- Neu data migration can → chay rieng

## Validation Checklist
- [ ] Schema dung sau migration
- [ ] No data loss
- [ ] Critical queries work

## Failure Cases
- Migration fail giua chung → partial state

## Recovery Actions
- Rollback tu backup, fix script, chay lai

## Completion Criteria
- Migration done
- Schema verified

## Next-Step Handoff
- T412 (Deploy & SelfTest)

## Reusable Prompt Block
```text
Step 1 Migration:

 "You are a senior database architect specialized in safe production migrations (MySQL 8.0).

Your task is to generate a database migration script.

INPUT:

1. Architectural Pack : architecturepack_TEP230_V4.3_25032026

2. IncrementalStepPlan: IncrementalStepPlan-TEP230V4.3_26032026

3. Vault file → database connection info (schema name must be extracted)

4. Current database schema → actual structure of tables, columns, indexes



TASK:

- Read Architectural Pack deeply and IncrementalStepPlan to find all migration step needed.

- Compare new requirements with the current schema

- Identify necessary changes:

  + Add / alter columns

  + Create new tables

  + Add indexes / constraints

- DO NOT remove or drop anything unless explicitly required

- Ensure backward compatibility and data safety



OUTPUT:

Return ONLY a single SQL file named `migration.sql`



STRICT FORMAT REQUIREMENTS:



1. Header:

- Include migration name, version, database name, date

- Example style:

  -- =====================================================

  -- <Project/Module> Migration <version>

  -- Database: <db_name>

  -- Date: <dd/mm/yyyy>

  -- =====================================================



2. Each change MUST be grouped into STEPS:

- Step title format:

  -- =====================================================

  -- Step X: <description>

  -- =====================================================



3. BEFORE each schema change:

- Provide a CHECK query using information_schema

- Example:

  -- CHECK: SELECT COUNT(*) FROM information_schema.COLUMNS WHERE ...



4. Conditional execution:

- DO NOT use IF statements

- Instead:

  + Write comment: "-- If result = 0, run:"

  + Then provide SQL



5. SQL rules:

- Compatible with MySQL 8.0

- Prefer safe operations:

  + ADD COLUMN with DEFAULT NULL

  + CREATE TABLE IF NOT EXISTS

- Always add indexes if relevant

- Use meaningful index names



6. Comments:

- Explain purpose of each change clearly

- Keep concise, production-style



7. Verification section (MANDATORY at the end):

- Provide queries to verify all changes:

  + Column existence

  + Table existence

  + Index existence (if needed)



8. Idempotency:

- Script should be safe to re-check manually

- Avoid destructive operations



9. Output constraints:

- DO NOT include explanations outside SQL

- DO NOT include markdown

- ONLY raw SQL text



IF no changes are required:

Return exactly:

No migration needed

----------
```
