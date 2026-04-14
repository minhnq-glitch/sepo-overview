---
name: sepo-t110
description: "T110 - L1: Voi tong — Entity Overview"
---

# Skill: T110 — L1: Voi tong — Entity Overview

**Expert Team:** Business Analyst + System Architect

## Objective
Phan tich he thong va tao document L1 (Voi tong): tong quan cac entities, relationships, va system boundary.

**Scope:** Business Analysis, System Architecture, Entity modeling

## Trigger condition
Sau khi setup xong (T010). Keywords: "voi tong", "L1", "tong quan he thong", "entity overview"

## Preconditions
- Du an da setup (T010 done)
- Co tai lieu yeu cau (VOI, feature doc, hoac existing codebase)
- Cac file input da duoc convert sang markdown (neu la Gsheet/Excel)

## Inputs
### Required
- Tai lieu mo ta he thong (VOI, feature request, hoac codebase existing)
- Format output mong muon: VoiTable
### Optional
- Existing L1 tu version truoc (de update, khong tao moi)
- Domain knowledge files (entity layers, glossary)

## Output Contract
- File L1_VoiTong.md theo format VoiTable
- Liet ke TAT CA entities voi: ten, mo ta, relationships
- Entity layer classification (A: Reference, B: Plan, C: Transaction)
- System boundary diagram (ASCII)

## Internal Reasoning Checklist
- [ ] He thong nay co nhung domain nao?
- [ ] Moi domain co nhung entity chinh nao?
- [ ] Entity nao la master data (it thay doi)?
- [ ] Entity nao la transaction data (thay doi thuong xuyen)?
- [ ] Relationships giua cac entity la gi (1-1, 1-N, N-N)?
- [ ] Co event/message passing giua cac entity khong?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `VOI / Requirement docs` | title chứa `VOI` | Nếu local chưa có → download |

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
#    - Filter: title chứa "VOI"
#    - Check local: ls **/*VOI* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `L1_VoiTong.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@L1_VoiTong.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"L1_VoiTong.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T110 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T110") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc TAT CA tai lieu input (VOI, docs, hoac scan codebase)
2. Xac dinh cac domain/module chinh cua he thong
3. Liet ke entities trong moi domain
4. Phan loai entity theo layer: A (Reference/Master), B (Plan/Config), C (Transaction)
5. Ve relationships giua cac entities
6. Tao system boundary diagram (ASCII)
7. Output: L1_VoiTong.md theo format VoiTable
8. Self-review: da cover het entities chua? co thieu relationship nao khong?

## Branching Logic
- Neu he thong moi hoan toan → tao L1 tu dau dua tren VOI
- Neu he thong da co → scan codebase (DB schema, models) de extract entities
- Neu co nhieu module → tao L1 cho tung module, roi merge

## Validation Checklist
- [ ] Moi entity co ten + mo ta + layer classification
- [ ] Moi relationship co cardinality (1-1, 1-N, N-N)
- [ ] System boundary diagram hien thi dung
- [ ] Khong co entity mo co (orphan entity khong relationship)
- [ ] Entity names nhat quan (khong duplicate, khong typo)

## Failure Cases
- Tai lieu input khong du → khong the xac dinh het entities
- Entity relationships khong ro → mo ho ve cardinality
- Domain qua lon → L1 qua dai, kho doc

## Recovery Actions
- Thieu tai lieu → hoi user cung cap them, hoac scan codebase
- Relationship khong ro → danh dau [NEEDS CLARIFICATION] va hoi user
- Domain lon → chia nho theo module, tao sub-L1s

## Completion Criteria
- File L1_VoiTong.md da tao
- TAT CA entities da liet ke voi layer classification
- Relationships da ve
- User da review va confirm

## Next-Step Handoff
- T120 (L2: Cac Workflow) — chi tiet hoa cac luong nghiep vu

## Reusable Prompt Block
```text
Chuyển các File dưới đây thành file dạng MarkDown
```
