---
name: sepo-t412
description: "T412 - Deploy & SelfTest tai Local Host"
---

# Skill: T412 — Deploy & SelfTest tai Local Host

**Expert Team:** DevOps Engineer + QA Engineer

## Objective
Deploy app len local, chay smoke test de verify basic functionality.

**Scope:** Deployment, Smoke Testing

## Trigger condition
Sau migration (T411). Keywords: "deploy local", "selftest"

## Preconditions
- Migration done (T411)
- Code da build

## Inputs
### Required
- Codebase
- .env file
- DB with migrated schema
### Optional

## Output Contract
- App running tai localhost
- Health check pass
- Basic flows work

## Internal Reasoning Checklist
- [ ] Server start ok?
- [ ] Health endpoint responds?
- [ ] Can login?

## 📥 Download từ SEPO (trước khi execute)

> Step này **không cần download docs** từ SEPO.

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. npm install (neu can)
2. npm run build
3. Start server: npm run dev
4. Verify health: curl localhost:PORT/health
5. Smoke test: login, navigate main pages, basic CRUD
6. Output: selftest results

## Branching Logic
- Neu server fail start → check logs, fix
- Neu smoke test fail → fix code, re-deploy

## Validation Checklist
- [ ] Health check 200
- [ ] Login works
- [ ] Main pages load

## Failure Cases
- Server crash
- DB connection error at runtime

## Recovery Actions
- Check .env
- Check logs
- Restart server

## Completion Criteria
- App running
- Smoke test pass

## Next-Step Handoff
- T413 (Push Git)

## Reusable Prompt Block
```text
Step 1: Migration:
 "You are a senior database architect specialized in safe production migrations (MySQL 8.0).
Your task is to generate a database migration script.
INPUT:
1. Architectural Pack : [architecturepack_version_date]
2. IncrementalStepPlan: [IncrementalStepPlan_version_date]
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
Step 2: Deploy app lên localhost, tạo các file env từ vault nếu cần.
```

## Examples
```text
Đây là output mẫu cho Step 1: Migration:
"-- =====================================================
-- TEP230 Audit v4.1.9 Migration (MySQL 8.0 compatible)
-- Database: staging_s2_bp_log_v2 @ mysql.clevai.vn
-- Date: 24/03/2026
-- Run manually with DBA account (needs ALTER permission)
-- =====================================================

-- =====================================================
-- Step 1: CHPT - add audit_type column
-- (SKIP if column already exists — check first)
-- =====================================================
-- CHECK: SELECT COUNT(*) FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='staging_s2_bp_log_v2' AND TABLE_NAME='bp_chpt_checkprocesstemp' AND COLUMN_NAME='audit_type';
-- If result = 0, run:
ALTER TABLE bp_chpt_checkprocesstemp
  ADD COLUMN audit_type VARCHAR(20) DEFAULT NULL COMMENT 'Audit type: ONB, HOT, WKL, MTL, RTR';

CREATE INDEX idx_chpt_audit_type ON bp_chpt_checkprocesstemp (audit_type);

-- =====================================================
-- Step 2: CHPI - add 8 retrain fields
-- (SKIP if columns already exist)
-- =====================================================
-- CHECK: SELECT COUNT(*) FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='staging_s2_bp_log_v2' AND TABLE_NAME='bp_chpi_checkprocessitem' AND COLUMN_NAME='retrain_trainer';
-- If result = 0, run:
ALTER TABLE bp_chpi_checkprocessitem
  ADD COLUMN retrain_trainer VARCHAR(128) DEFAULT NULL,
  ADD COLUMN retrain_status VARCHAR(32) DEFAULT NULL,
  ADD COLUMN retrain_date DATE DEFAULT NULL,
  ADD COLUMN retrain_result VARCHAR(32) DEFAULT NULL,
  ADD COLUMN retrain_trained_at DATETIME DEFAULT NULL,
  ADD COLUMN retrain_notes TEXT DEFAULT NULL,
  ADD COLUMN retrain_failed_items JSON DEFAULT NULL,
  ADD COLUMN retrain_reaudit_chpi VARCHAR(512) DEFAULT NULL;

CREATE INDEX idx_chpi_retrain_status ON bp_chpi_checkprocessitem (retrain_status);
CREATE INDEX idx_chpi_retrain_trainer ON bp_chpi_checkprocessitem (retrain_trainer);

-- =====================================================
-- Step 3: bp_setting table (replaces audit_threshold_config)
-- =====================================================
CREATE TABLE IF NOT EXISTS bp_setting (
  id BIGINT AUTO_INCREMENT PRIMARY KEY,
  pt VARCHAR(128) DEFAULT NULL,
  entitycode VARCHAR(128) NOT NULL,
  attributecode VARCHAR(128) NOT NULL,
  attributevalue TEXT DEFAULT NULL,
  published TINYINT DEFAULT 1,
  description VARCHAR(512) DEFAULT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP,
  INDEX idx_setting_entity (entitycode),
  INDEX idx_setting_attr (entitycode, attributecode)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- =====================================================
-- VERIFICATION QUERIES (run after migration)
-- =====================================================
-- SELECT COLUMN_NAME FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='staging_s2_bp_log_v2' AND TABLE_NAME='bp_chpt_checkprocesstemp' AND COLUMN_NAME='audit_type';
-- SELECT COUNT(*) FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='staging_s2_bp_log_v2' AND TABLE_NAME='bp_chpi_checkprocessitem' AND COLUMN_NAME LIKE 'retrain%';
-- SHOW TABLES LIKE 'bp_setting';"

```
