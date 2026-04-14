---
name: sepo-helper-download
description: "Helper: Download docs từ SEPO Web cho step hiện tại"
---

# Helper: Download Docs từ SEPO

**Loại:** Helper (skill bổ trợ, tái sử dụng bởi nhiều steps)

## Mục đích
Tải docs cần thiết từ SEPO Web về local khi chạy 1 step.

## Khi nào dùng
- Bất kỳ step nào cần input docs từ SEPO (T110, T120, T130, T212, T220, ...)
- Claude gọi helper này trước khi execute skill chính

## Input
- `stepCode` — step hiện tại (VD: "T212")
- `projectId` — project đang làm
- `baseUrl` — SEPO API URL (default: https://sepo.mikai.tech)

## Execution

```bash
# 1. Login
curl -s -c cookies.txt -X POST \
  -H "Content-Type: application/json" \
  -d '{"email":"tuanpm@clevai.edu.vn"}' \
  ${BASE_URL}/api/dev/quick-login

# 2. Get task-config by code
TASK_CONFIG_ID=$(curl -s -b cookies.txt ${BASE_URL}/api/task-configs \
  | jq -r ".[] | select(.code==\"${STEP_CODE}\") | .id")

# 3. Get prompt + required doc IDs
curl -s -b cookies.txt \
  ${BASE_URL}/api/prompt-configs/by-task-config/${TASK_CONFIG_ID} \
  > /tmp/prompt.json

# 4. For each docId in docIds[]:
#    - Check if local file exists (match by title)
#    - If not: GET /api/docs/{docId}/versions → contentText → save to docs/
#    - Report: "Downloaded: ${title}"
```

## Docs mapping theo step

| Step | Keywords trong title |
|------|---------------------|
| T110 | VOI, requirement |
| T120 | L1, VoiTong |
| T130 | L1, L2, Workflow |
| T212 | L1, L2, L3, schema, layerevent |
| T220 | ArchitecturePack |
| T230 | IncrementalStepPlan, ArchitecturePack |
| T310 | ISP, Audited |
| T390 | ArchitecturePack |
| T422 | TestFeedback, template |

## Output
- Files saved vào `docs/` folder
- Report list of files downloaded
- Claude tiếp tục với skill chính

## Errors
- **401 Unauthorized** → Re-login, retry
- **404 Not found** → Hỏi user: "Doc không có trên SEPO, bạn có muốn tạo không?"
- **Network error** → Retry 3 lần, then báo user
