---
name: sepo-helper-detect
description: "Helper: Phát hiện user đang ở step nào dựa trên keywords + workspace context"
---

# Helper: Detect Current Step

**Loại:** Helper (skill bổ trợ, dùng bởi SKILL.md master)

## Mục đích
Phân tích input của user + workspace để xác định step SEPO đang cần chạy.

## Khi nào dùng
- User nhập prompt mở trong skill `/sepo` (không có step code cụ thể)
- Cần suggest "step tiếp theo" dựa trên workspace state

## Input
- `userPrompt` — nội dung user gõ
- `workspacePath` — thư mục project hiện tại (để scan files)

## Logic

### Step 1: Match theo keyword

Scan `userPrompt` với bảng:

| Keyword (case-insensitive) | Step |
|----------------------------|------|
| "setup", "clone", "pull git", "kết nối db" | T010 |
| "vẽ voi", "l1", "voi tổng" | T110 |
| "workflow", "l2", "luồng" | T120 |
| "l3", "chi tiết workflow", "cep" | T130 |
| "project guide" | T140 |
| "db schema", "migration prep" | T211 |
| "architecture pack", "build ap", "kiến trúc" | T212 |
| "isp", "incremental step", "step plan" | T220 |
| "audit isp" | T230 |
| "build step", "implement", "build prompt" | T310-001 |
| "test prompt", "chạy test" | T320-001 |
| "hoàn thành isp" | T380 |
| "eval", "post-audit" | T390 |
| "migration", "run migration" | T411 |
| "deploy local", "selftest" | T412 |
| "push git", "commit" | T413 |
| "deploy staging" | T414 |
| "checkpoint stg" | T415 |
| "chuẩn bị test", "testcase" | T421 |
| "uat", "test feedback" | T422 |
| "classify feedback" | T423 |
| "task a", "ui fix" | T431 |
| "task b", "business logic" | T432 |
| "task c", "performance" | T433 |
| "deploy production" | T510 |
| "checkpoint prd" | T530 |

### Step 2: Nếu không match → Scan workspace

| Tìm thấy file | Suy ra step tiếp theo |
|----------------|----------------------|
| Không có gì | → T010 (setup) |
| `.git/`, `.env`, `package.json` | → T110 (bắt đầu analysis) |
| `*VoiTong*`, `*L1*` | → T120 |
| `*Workflow*`, `*L2*` | → T130 |
| `*L3*`, `*WorkflowDetail*` | → T140 hoặc T211 |
| `*ArchitecturePack*` | → T220 |
| `*IncrementalStepPlan*` (no "Audited") | → T230 |
| `*Audited*` | → T310-001 (bắt đầu build) |
| `*PostAudit*` | → T411 (bắt đầu deploy) |
| `*STG_VersionCommit*` | → T421 (UAT) |
| `*TestFeedback*` | → T423 (classify) |
| `*PRD_VersionCommit*` | DONE! |

### Step 3: Nếu ambiguous (>1 match)

Hiển thị top 3 matches, hỏi user confirm:

```
Có thể bạn muốn:
  1. T212 - Build ArchitecturePack (confidence: 0.85)
  2. T220 - Tạo Incremental Step Plan (confidence: 0.60)
  3. T230 - Audit ISP (confidence: 0.45)

Chọn step nào? [1/2/3]
```

## Output

```json
{
  "detectedStep": "T212",
  "confidence": 0.9,
  "source": "keyword",  // or "workspace" or "explicit"
  "alternatives": ["T220", "T230"]
}
```

## Integration với SKILL.md master

```
User: "Giúp tôi build architecture pack"
   ↓
Master gọi helper: sepo-helper-detect
   ↓ { detectedStep: "T212", confidence: 0.9 }
Master gọi helper: sepo-helper-download(T212)
   ↓ Downloads L1, L2, L3, schema
Master đọc skill: steps/T2/T210/T212.md
   ↓ Execute prompt
Master gọi helper: sepo-helper-upload(T212, output.md)
   ↓ Upload + mark Done
```
