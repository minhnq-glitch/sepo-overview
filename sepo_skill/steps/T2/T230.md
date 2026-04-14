---
name: sepo-t230
description: "T230 - Audit Incremental Steps Plans"
---

# Skill: T230 — Audit Incremental Steps Plans

**Expert Team:** ISP Quality Auditor + Software Architect

## Objective
Kiem tra chat luong ISP: coverage, clarity, test criteria, va enrich voi data/API/UI details.

**Scope:** Quality Audit, Plan Review

## Trigger condition
Sau T220. Keywords: "audit ISP", "kiem tra plan", "review plan"

## Preconditions
- ISP (T220) da tao
- AP da approved

## Inputs
### Required
- IncrementalStepPlan.md
- ArchitecturePack.md
### Optional
- Previous audit results

## Output Contract
- IncrementalStepPlanAudited.md:
-   - Moi step da enriched voi chi tiet
-   - Coverage matrix: AP section → ISP step
-   - Gaps identified va resolved
-   - Status: ENRICH (ok) hoac BLOCK (can fix)

## Internal Reasoning Checklist
- [ ] Moi step co scope ro rang?
- [ ] Test criteria cu the (khong mo ho)?
- [ ] Files listed chinh xac?
- [ ] Dependencies dung?
- [ ] Co feature nao trong AP ma ISP chua cover?

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `IncrementalStepPlan` | title chứa `IncrementalStepPlan` | Nếu local chưa có → download |
| `ArchitecturePack` | title chứa `ArchitecturePack` | Nếu local chưa có → download |

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
#    - Filter: title chứa "IncrementalStepPlan"
#    - Check local: ls **/*IncrementalStepPlan* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "ArchitecturePack"
#    - Check local: ls **/*ArchitecturePack* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

**Output files có thể upload:**

- 📄 `IncrementalStepPlanAudited_{project}_{version}.md`

**Procedure:**
```bash
# 1. Hỏi user: "Đã tạo xong [file]. Upload lên SEPO không?"
# 2. Nếu user đồng ý:
BASE=${SEPO_BASE_URL:-https://sepo.mikai.tech}

# Option A: Upload file (multipart/form-data)
curl -X POST ${BASE}/api/doc/upload-with-match \
  -b cookies.txt \
  -F "file=@IncrementalStepPlanAudited_{project}_{version}.md" \
  -F "projectId=${PROJECT_ID}"

# Option B: Create doc + version (JSON)
# DOC_ID=$(curl -X POST ${BASE}/api/docs -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"projectId":"${PROJECT_ID}","title":"IncrementalStepPlanAudited_{project}_{version}.md","category":"Project","docFormat":"md"}' | jq -r .id)
# curl -X POST ${BASE}/api/docs/${DOC_ID}/versions -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"contentText":"<content>","changeNote":"T230 output"}'

# 3. Update task-item status → Done
# TASK_ITEM_ID=$(curl -s "${BASE}/api/task-items?projectId=${PROJECT_ID}" -b cookies.txt | \
#   jq -r '.[] | select(.taskConfigCode=="T230") | .id' | head -1)
# curl -X PATCH ${BASE}/api/task-items/${TASK_ITEM_ID} -b cookies.txt \
#   -H "Content-Type: application/json" \
#   -d '{"doneStatus":"Done","completedAt":"<ISO timestamp>"}'
```

## Execution Procedure
1. Doc ISP + AP song song
2. Voi moi step trong ISP:
3.   - Scope ro rang khong? → enrich neu can
4.   - Test criteria cu the? → them test cases
5.   - Files listed? → verify vs codebase
6.   - Dependencies dung? → check
7. Tao coverage matrix: AP section → ISP step
8. Tim gaps: AP features khong co step → them step
9. Classify: ENRICH (ok) hoac BLOCK (can sua)
10. Output: IncrementalStepPlanAudited.md

## Branching Logic
- Neu gap nhieu BLOCK steps → bao user truoc khi tiep tuc
- Neu coverage < 90% → can them steps

## Validation Checklist
- [ ] Coverage >= 95% (AP → ISP)
- [ ] Khong co BLOCK step nao con lai
- [ ] Moi step co test criteria cu the

## Failure Cases
- Coverage thap → se co features khong duoc build

## Recovery Actions
- Them steps cho features missing
- Lam lai T220 neu ISP qua kem

## Completion Criteria
- Audited ISP created
- 95%+ coverage
- No BLOCK steps remaining

## Next-Step Handoff
- T3 (Build BApp) — bat dau code

## Reusable Prompt Block
```text
# Vai trò
Bạn là ISP Quality Auditor. Nhiệm vụ: kiểm tra từng ISP step có đủ detail
để một AI coding agent (Claude Code) implement chính xác hay không.

Bạn có kinh nghiệm thực tế với các lỗi thường gặp khi ISP thiếu detail:
- AI viết stub/placeholder thay vì code thật
- AI dùng string concat thay vì library đúng (xlsx, PDF)
- AI implement FE nhưng bỏ sót BE (hoặc ngược lại)
- AI đánh Done nhưng chỉ verify 1 chiều
- AI không biết data source nằm ở đâu (table nào, API nào, hệ thống ngoài nào)
- AI implement theo cách đơn giản nhất thay vì theo wireframe chính xác

# Input

## Architectural Pack (AP)
Đây là nguồn truth cho business logic, wireframe, algorithm.
<AP>
{Paste AP sections liên quan — wireframe, algorithm, constraints}
</AP>

## ISP Steps file audit
<ISP>


## Tech stack
<Clevai_Tech_Stack_Prefer>


# Quy trình audit

Với MỖI ISP step, kiểm tra 7 tiêu chí dưới đây. Mỗi tiêu chí đánh: ✅ Có / ❌ Thiếu

## 7 TIÊU CHÍ BẮT BUỘC

### 1. Data Source rõ ràng
- Ghi rõ table name + field name trong database
- Nếu data từ hệ thống ngoài → ghi rõ tên hệ thống, API endpoint, format response
- Nếu data cần JOIN → ghi rõ JOIN condition
- ❌ FAIL nếu: chỉ ghi "lấy data từ DB" mà không nói table/field nào

### 2. API Contract đầy đủ
- Có HTTP method + endpoint path
- Có request params/body schema
- Có response schema (field name + type)
- Có error cases (4xx, 5xx)
- ❌ FAIL nếu: chỉ ghi "tạo API" mà không có endpoint + schema

### 3. UI Spec cụ thể
- Có wireframe reference (AP section number)
- Mô tả layout: component nào, vị trí nào, format hiển thị nào
- Format output rõ ràng (ví dụ: "fullname (usiCode)" thay vì "hiện thông tin teacher")
- Interaction: click/hover/select behavior
- ❌ FAIL nếu: chỉ ghi "hiển thị data" mà không nói format/layout

### 4. Business Logic / Algorithm
- Có công thức tính toán (nếu có)
- Có threshold/mapping (nếu có)
- Có flow điều kiện (if/else logic)
- Reference đến AP algorithm number
- ❌ FAIL nếu: có tính toán nhưng không ghi công thức cụ thể

### 5. Acceptance Criteria dạng Checklist
- Mỗi criterion là 1 câu có thể verify TRUE/FALSE
- Cover cả FE lẫn BE (không chỉ 1 phía)
- Có test case cụ thể với data mẫu
- Có edge case (empty state, error state, null value)
- ❌ FAIL nếu: không có acceptance criteria, hoặc chỉ có 1 dòng chung chung

### 6. Library / Tool chỉ định (nếu step có export/generate)
- Nếu export Excel → chỉ định library (excelize cho Go, xlsx cho JS)
- Nếu generate PDF → chỉ định library + font Vietnamese
- Nếu upload file → chỉ định format, size limit, parse method
- ❌ FAIL nếu: có export/generate nhưng không chỉ định library
  (AI SẼ dùng string concat nếu không chỉ định!)

### 7. Dependencies & Sequence
- Step này depend on step nào khác?
- Data input từ step nào?
- Có cần deploy/migration trước không?
- ❌ FAIL nếu: step cần data từ feature khác chưa implement mà không ghi rõ

# Output format

Trả về đúng format sau:

## Bảng Audit Tổng Hợp

| Step | Tên | ① Data | ② API | ③ UI | ④ Logic | ⑤ AC | ⑥ Lib | ⑦ Deps | Verdict |
|------|-----|--------|-------|------|---------|------|-------|--------|---------|
| X.Y  | ... | ✅/❌  | ✅/❌ | ✅/❌| ✅/❌  | ✅/❌| ✅/❌ | ✅/❌  | PASS/ENRICH/BLOCK |

## Verdict rules:
- **✅ PASS** (7/7 tiêu chí đạt) → Step đủ detail, đưa thẳng vào Claude Code
- **⚠️ ENRICH** (có tiêu chí thiếu NHƯNG tìm được trong AP) → Bạn tự bổ sung detail từ AP, output step đã enrich
- **❌ BLOCK** (có tiêu chí thiếu VÀ AP cũng không có) → Liệt kê CÂU HỎI CỤ THỂ cần user trả lời

## Chi tiết từng step

### Step X.Y — [Tên] → [Verdict]

**Thiếu:**
- [Liệt kê tiêu chí nào thiếu]

**Đã bổ sung từ AP:** (nếu ENRICH)
- [Thông tin đã tìm được từ AP sections nào]

**Câu hỏi cho user:** (nếu BLOCK)
- [Câu hỏi cụ thể, không chung chung]
- Ví dụ ĐÚng: "Video URL nằm ở table nào? Hay từ API hệ thống SCS? Nếu SCS thì endpoint là gì?"
- Ví dụ SAI: "Cần thêm thông tin về video"

## ISP Step đã Enrich (nếu có)

Với mỗi step ENRICH, output lại step hoàn chỉnh theo format ISP mẫu,
đã bổ sung đầy đủ 7 tiêu chí. Đây là bản sẵn sàng đưa vào Claude Code.

#Output file:
 Tạo ra file : IncrementalStepPlanAudited_[VersionofArchitecturalPack]_[DateofArchitecturalPack]

# Lưu ý quan trọng

1. KHÔNG BAO GIỜ đánh PASS cho step chỉ có tên mà không có detail
   (đây là nguyên nhân 62% bugs trong dự án thực tế)

2. Nếu step có export/generate mà không chỉ định library →
   tự động BLOCK hoặc ENRICH bằng cách thêm library recommendation

3. Nếu step có cả FE + BE mà acceptance criteria chỉ cover 1 phía →
   tự động ENRICH thêm criteria cho phía còn lại

4. Nếu step reference "theo wireframe" mà không ghi format cụ thể →
   tìm trong AP wireframe section và ENRICH format rõ ràng
   (ví dụ: không chấp nhận "hiện teacher info", phải là "fullname (usiCode)")

5. Với mỗi step BLOCK, câu hỏi phải đủ cụ thể để user trả lời trong 1-2 câu
   Không hỏi chung chung kiểu "cần thêm detail"
```
