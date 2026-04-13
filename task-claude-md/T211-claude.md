# CLAUDE.md — Task T211: Prep DB Schema & Migration

## Task Identity
- **Code:** T211
- **Name:** Prep: Update DB Schema
- **Phase:** T2 — Architecture Pack & ISP
- **Classification:** Type B (schema change, needs Team Lead approval)
- **Expert Team:** Database Architect + Backend Developer

## Objective
Phan tich entities tu L1/AP va de xuat/update DB schema. Sinh migration SQL neu can.

## Preconditions
- L1 da co (hoac AP draft)
- Biet DB engine (PostgreSQL/MySQL)

## Input Docs (Required)
- **L1_VoiTong.md** hoac ArchitecturePack draft
- **Current DB schema** (neu co)

## Output Contract
- DB schema proposal: tables, columns, types, constraints
- **migration.sql** (neu can thay doi schema)
- ERD diagram (ASCII)

## Instructions

Ban la Database Architect. Nhiem vu: phan tich entities va de xuat DB schema.

### Execution Steps:
1. Doc L1_VoiTong.md de hieu entities va relationships
2. Map entities sang database tables
3. Xac dinh columns, types, constraints, indexes
4. So sanh voi current schema (neu co) de tim differences
5. Tao migration.sql cho cac thay doi
6. Ve ERD diagram (ASCII)

### Schema Design Rules:
- UUID cho primary keys
- `created_at`, `updated_at` timestamps cho moi table
- Soft delete voi `deleted_at` nullable timestamp
- Foreign keys co `ON DELETE` policy ro rang
- Indexes cho cac columns thuong query

## Validation Checklist
- [ ] Tat ca entities da duoc map sang tables
- [ ] Migration SQL chay khong loi
- [ ] ERD diagram phan anh dung schema
- [ ] Backward compatible (khong mat data)

## Next Step
T212 — Build ArchitecturePack

## References
- @docs/L1_VoiTong.md — load for entity definitions
- @docs/database_schema_stg.md — load for current schema reference
