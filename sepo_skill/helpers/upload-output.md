---
name: sepo-helper-upload
description: "Helper: Upload output doc lên SEPO Web sau khi hoàn thành step"
---

# Helper: Upload Output lên SEPO

**Loại:** Helper (skill bổ trợ, tái sử dụng)

## Mục đích
Upload file output (VD: ArchitecturePack.md, ISP.md...) lên SEPO Web sau khi step chạy xong.

## Khi nào dùng
- Sau khi execute skill chính tạo ra file output
- Trước khi đánh dấu step "Done"

## Input
- `stepCode` — step vừa chạy
- `outputFilePath` — đường dẫn file output local
- `projectId` — project
- `title` — title cho doc trên SEPO

## Execution

```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không? (y/n)"
# Nếu n → SKIP, giữ file local

# 2. Check doc đã tồn tại trên SEPO chưa
EXISTING=$(curl -s -b cookies.txt \
  "${BASE_URL}/api/docs?projectId=${PROJECT_ID}" \
  | jq -r ".[] | select(.title==\"${TITLE}\") | .id" | head -1)

# 3a. Nếu đã có → tạo version mới
if [ -n "$EXISTING" ]; then
  curl -s -b cookies.txt -X POST \
    -H "Content-Type: application/json" \
    -d "{\"contentType\":\"text\",\"contentText\":$(cat file.md | jq -Rs),\"changeNote\":\"Update from ${STEP_CODE}\"}" \
    ${BASE_URL}/api/docs/${EXISTING}/versions

# 3b. Nếu chưa có → tạo doc mới
else
  DOC_ID=$(curl -s -b cookies.txt -X POST \
    -H "Content-Type: application/json" \
    -d "{\"projectId\":\"${PROJECT_ID}\",\"title\":\"${TITLE}\",\"category\":\"Project\",\"docFormat\":\"TextBlock\",\"createdByRole\":\"admin\"}" \
    ${BASE_URL}/api/docs | jq -r '.id')

  curl -s -b cookies.txt -X POST \
    ${BASE_URL}/api/docs/${DOC_ID}/versions \
    -H "Content-Type: application/json" \
    -d "{\"contentType\":\"text\",\"contentText\":$(cat file.md | jq -Rs),\"changeNote\":\"Initial from ${STEP_CODE}\"}"
fi

# 4. Update task-item status → Done
TASK_ITEM_ID=$(curl -s -b cookies.txt \
  "${BASE_URL}/api/task-items?projectId=${PROJECT_ID}" \
  | jq -r ".[] | select(.taskConfigCode==\"${STEP_CODE}\") | .id" | head -1)

curl -s -b cookies.txt -X PATCH \
  "${BASE_URL}/api/task-items/${TASK_ITEM_ID}" \
  -H "Content-Type: application/json" \
  -d '{"doneStatus":"Done"}'
```

## Output mapping theo step

| Step | Output file | Title trên SEPO |
|------|-------------|-----------------|
| T110 | L1_VoiTong.md | "L1: Voi tong" |
| T120 | L2_Workflows.md | "L2: Workflows" |
| T130 | L3_WorkflowDetails.md | "L3: Workflow Details" |
| T212 | ArchitecturePack_{ver}.md | "ArchitecturePack v{ver}" |
| T220 | IncrementalStepPlan_{ver}.md | "IncrementalStepPlan v{ver}" |
| T230 | ISPAudited_{ver}.md | "ISP Audited v{ver}" |
| T390 | PostAudit_{ver}.md | "PostAudit v{ver}" |
| T415 | STG_VersionCommit.md | "STG VersionCommit" |
| T422 | TestFeedback_{date}.xlsx | "TestFeedback {date}" |
| T530 | PRD_VersionCommit.md | "PRD VersionCommit" |

## Confirmation UX

```
📤 Upload Output
   File: ArchitecturePack_v2.2.3.md (45 KB)
   Title: "ArchitecturePack v2.2.3"
   Project: PO Test Project
   Action: Create new doc (title chưa tồn tại)

   Upload? [y/n]
```

## Errors
- **409 Conflict** → Doc đã có version giống → bỏ qua hoặc tạo version mới
- **413 Payload too large** → File quá lớn → nén hoặc split
- **403 Forbidden** → Không có permission → hỏi admin
