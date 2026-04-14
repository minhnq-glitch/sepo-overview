---
name: sepo
description: "SEPO Process Orchestrator — Mỗi step: detect → check docs local → download từ SEPO nếu thiếu → execute skill → hỏi upload output. Use when: user mentions build step (T010-T530), build app, deploy, test, architecture, ISP, migration, UAT."
user_invocable: true
---

# SEPO Skill — Master Orchestrator

## CORE FLOW — Chạy mỗi khi user bắt đầu 1 step

```
USER INPUT
    │
    ▼
[1] DETECT STEP → xác định step code (T010, T212, T310-001...)
    │
    ▼
[2] CHECK LOCAL → scan workspace, kiểm tra docs cần có cho step
    │
    ├── Có đủ → tiếp tục
    └── Thiếu → DOWNLOAD từ SEPO Web API
    │
    ▼
[3] EXECUTE → đọc skill file, chạy prompt, output kết quả
    │
    ▼
[4] UPLOAD → hỏi user "Upload output lên SEPO không?"
```

---

## [1] DETECT STEP

### Từ keyword

| User nói | Step |
|----------|------|
| setup, clone, pull git, kết nối DB | T010 |
| vẽ voi, L1, tổng quan entity | T110 |
| workflow, L2, luồng nghiệp vụ | T120 |
| L3, chi tiết workflow, CEP | T130 |
| project guide | T140 |
| DB schema, migration prep | T211 |
| architecture pack, build AP | T212 |
| ISP, step plan, incremental step | T220 |
| audit ISP, kiểm tra plan | T230 |
| build step, implement, build prompt | T310-001 |
| test prompt, chạy test, verify | T320-001 |
| hoàn thành ISP, finish plan | T380 |
| eval, đánh giá, audit code | T390 |
| migration, run migration | T411 |
| deploy local, selftest | T412 |
| push git, commit | T413 |
| deploy staging | T414 |
| checkpoint STG | T415 |
| chuẩn bị test, testcase | T421 |
| UAT, test feedback | T422 |
| classify feedback, A/B/C | T423 |
| task A, UI fix | T431 |
| task B, business logic | T432 |
| task C, performance | T433 |
| deploy production | T510 |
| checkpoint PRD | T530 |

### Từ context (scan workspace)

| Tìm thấy file | Suy ra đang ở | Suggest next |
|----------------|---------------|--------------|
| Không có gì | Chưa bắt đầu | → T010 |
| Có repos, có .env | T010 done | → T110 |
| `*VoiTong*` hoặc `*L1*` | T110 done | → T120 |
| `*Workflow*` hoặc `*L2*` | T120 done | → T130 |
| `*L3*` hoặc `*WorkflowDetail*` | T130 done | → T212 |
| `*ArchitecturePack*` | T212 done | → T220 |
| `*IncrementalStepPlan*` (no Audited) | T220 done | → T230 |
| `*Audited*` | T230 done | → T310-001 |
| `*PostAudit*` | T390 done | → T411 |
| `*STG_VersionCommit*` | T415 done | → T421 |
| `*TestFeedback*` | T422 done | → T423 |
| `*PRD_VersionCommit*` | T530 done | DONE! |

### Từ SEPO API (task-item status)

```bash
# Check status các steps trên SEPO
curl -s ${BASE}/api/task-items?projectId=${PROJECT_ID} -b cookies.txt
```

Response chứa `doneStatus`: `Todo` | `Inprocess` | `Done` cho mỗi step.

---

## [2] CHECK LOCAL & DOWNLOAD

### SEPO API Reference

```bash
BASE=https://sepo.mikai.tech  # hoặc $SEPO_BASE_URL

# Login
curl -s -c cookies.txt -X POST \
  -H "Content-Type: application/json" \
  -d '{"email":"tuanpm@clevai.edu.vn"}' \
  ${BASE}/api/dev/quick-login

# Lấy projects
curl -s ${BASE}/api/projects -b cookies.txt

# Lấy task configs (danh sách steps)
curl -s ${BASE}/api/task-configs -b cookies.txt

# Lấy prompt của 1 step
curl -s ${BASE}/api/prompt-configs/by-task-config/${TASK_CONFIG_ID} -b cookies.txt
# Response: { systemPrompt, examples, docIds[], docSelectionRule }

# Lấy docs của project
curl -s "${BASE}/api/docs?projectId=${PROJECT_ID}" -b cookies.txt
# Response: [{ id, title, category, docLevel, docType, latestVersionId }]

# Lấy nội dung doc (versions)
curl -s ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt
# Response: [{ contentText, versionNo, changeNote }]
```

### Docs cần cho mỗi step

| Step | Docs cần có trước khi chạy | Cách lấy |
|------|---------------------------|----------|
| T010 | GIT-DB-Structure.md, vault.md | Có sẵn trong project hoặc download từ SEPO |
| T110 | VOI / requirement docs | GET /api/docs?projectId=X → filter title chứa "VOI" hoặc "requirement" |
| T120 | L1_VoiTong.md | GET /api/docs → filter title chứa "L1" hoặc "VoiTong" |
| T130 | L1 + L2 docs | GET /api/docs → filter L1, L2 |
| T140 | L1, L2, L3 docs | GET /api/docs → filter L1, L2, L3 |
| T211 | AP draft hoặc L1 docs | GET /api/docs → filter "ArchitecturePack" hoặc "L1" |
| T212 | L1, L2, L3, DB schema, layerevent.md | GET /api/docs → filter theo title keywords |
| T220 | ArchitecturePack | GET /api/docs → filter "ArchitecturePack" |
| T230 | ISP + AP | GET /api/docs → filter "IncrementalStepPlan" + "ArchitecturePack" |
| T310-001 | ISP Audited + AP section | GET /api/docs → filter "Audited", GET prompt-config |
| T320-001 | ISP step + code vừa build | Local codebase + ISP doc |
| T390 | AP + full codebase | GET /api/docs → filter "ArchitecturePack" |
| T422 | TestFeedback template | GET /api/docs → filter "TestFeedback" hoặc "T720" |
| T423 | TestFeedback filled | GET /api/docs → filter "TestFeedback" |

### Download procedure

```python
# Pseudocode — Claude thực hiện khi chạy step
1. Login SEPO API
2. GET /api/docs?projectId=X
3. Filter docs theo bảng trên (by title keywords)
4. Với mỗi doc cần:
   a. Kiểm tra local: file đã tồn tại chưa? (glob search)
   b. Nếu chưa có:
      - GET /api/docs/{docId}/versions
      - Lấy contentText từ version mới nhất
      - Save vào docs/ folder
      - Báo user: "Đã tải [title] từ SEPO"
   c. Nếu đã có:
      - So sánh updatedAt với local file mtime
      - Nếu SEPO mới hơn → hỏi user: "SEPO có version mới hơn. Cập nhật không?"
```

---

## [3] EXECUTE SKILL

Đọc skill file theo cấu trúc phân tầng:

```
sepo_skill/steps/
├── T0/T010.md              ← step skill (18 sections)
├── T1/T110.md, T120.md...  
├── T2/T210/T211.md, T212.md | T220.md, T230.md
├── T3/T300-001/T310-001.md, T320-001.md | T380.md, T390.md
├── T4/T410/T411-T415.md | T420/T421-T423.md | T430/T431-T433.md
└── T5/T510.md, T530.md
```

Mỗi skill file chứa 18 sections:
- Objective, Trigger, Preconditions
- Required/Optional Inputs
- Output Contract
- Internal Reasoning Checklist
- Execution Procedure (step-by-step)
- Branching Logic
- Validation Checklist
- Failure Cases + Recovery
- Completion Criteria
- Next-Step Handoff
- Reusable Prompt Block (prompt gốc từ SEPO)

**Quy tắc: CHẠY XONG HOÀN TOÀN — KHÔNG DỪNG GIỮA CHỪNG**

---

## [4] UPLOAD OUTPUT

Sau khi execute xong, scan output và hỏi user:

| Step | Output file(s) | Upload API |
|------|----------------|-----------|
| T110 | L1_VoiTong.md | POST /api/docs + POST /api/docs/{id}/versions |
| T120 | L2_Workflows.md | " |
| T130 | L3_WorkflowDetails.md | " |
| T140 | ProjectGuide.md | " |
| T211 | migration.sql | " |
| T212 | ArchitecturePack_{ver}.md | " |
| T220 | IncrementalStepPlan_{ver}.md | " |
| T230 | ISPAudited_{ver}.md | " |
| T390 | PostAudit_{ver}.md | " |
| T415 | STG_VersionCommit.md, STG_DB_Schema.md | " |
| T422 | TestFeedback_{date}.xlsx | POST /api/doc/upload-with-match |
| T530 | PRD_VersionCommit.md, PRD_DB_Schema.md | " |

### Upload procedure

```python
# Pseudocode
1. Tìm output file vừa tạo (theo Output Contract trong skill)
2. Hỏi user: "Đã hoàn thành [step]. Output: [file]. Upload lên SEPO không? (y/n)"
3. Nếu y:
   a. Kiểm tra doc đã tồn tại trên SEPO chưa (GET /api/docs?title=X)
   b. Nếu đã có → POST /api/docs/{docId}/versions (tạo version mới)
   c. Nếu chưa có → POST /api/docs (tạo doc mới) rồi POST version
   d. Báo: "Đã upload [file] lên SEPO (version X)"
4. Nếu n:
   - Báo: "OK, file giữ local. Bạn có thể upload sau bằng /sepo upload"
```

### Update task-item status

```bash
# Đánh dấu step đã done trên SEPO
curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} \
  -H "Content-Type: application/json" \
  -d '{"doneStatus":"Done"}' \
  -b cookies.txt
```

---

## COMMANDS

| Command | Hành vi |
|---------|---------|
| `/sepo` | Scan context → suggest step tiếp theo |
| `/sepo T212` | Chạy step T212 trực tiếp |
| `/sepo status` | GET task-items từ SEPO → show status tất cả steps |
| `/sepo download` | Tải tất cả docs của project từ SEPO về local |
| `/sepo upload` | Upload docs pending lên SEPO |

---

## PROCESS MAP

```
T0  Setup ──────────────────────────── Phase: Setup
  └─ T010  Setup ClaudeCode ✦

T1  Vẽ Voi ────────────────────────── Phase: Analysis
  ├─ T110  L1: Voi tổng ✦
  ├─ T120  L2: Workflows ✦
  ├─ T130  L3: Chi tiết ✦
  └─ T140  Project Guide ✦

T2  AP & ISP ──────────────────────── Phase: Architecture
  ├─ T210/
  │   ├─ T211  Prep DB Schema ✦
  │   └─ T212  Build AP ✦
  ├─ T220  Tạo ISP ✦
  └─ T230  Audit ISP ✦

T3  Build ──────────────────────────── Phase: Implementation
  ├─ T300-001/
  │   ├─ T310-001  Build Prompt ✦ (lặp mỗi ISP step)
  │   └─ T320-001  Test Prompt ✦ (lặp mỗi ISP step)
  ├─ T380  Hoàn thành ISP ✦
  └─ T390  Eval vs AP ✦

T4  Deploy & UAT ──────────────────── Phase: Deployment
  ├─ T410/ T411→T415 ✦
  ├─ T420/ T421→T423 ✦
  └─ T430/ T431→T433 ✦

T5  Production ────────────────────── Phase: Release
  ├─ T510  Deploy Prod ✦
  └─ T530  Checkpoint PRD ✦

✦ = có skill spec (18 sections + prompt SEPO)
```
