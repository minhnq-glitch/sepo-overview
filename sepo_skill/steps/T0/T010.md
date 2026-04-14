---
name: sepo-t010
description: "T010 - Setup ClaudeCode: Pull Git va ket noi DB"
---

# Skill: T010 — Setup ClaudeCode: Pull Git va ket noi DB

**Expert Team:** DevOps Engineer + Backend Developer

## Objective
Thiet lap moi truong phat trien: clone tat ca git repos lien quan va ket noi database de developer co the bat dau code ngay.

**Scope:** DevOps, Git, Database connection, Environment setup

## Trigger condition
User bat dau du an moi hoac setup may moi. Keywords: "setup", "clone", "pull git", "ket noi DB", "cai dat moi truong"

## Preconditions
- Co quyen truy cap GitLab (gitlab.clevai.vn)
- Co file GIT-DB-Structure.md trong project (chua thong tin repos)
- Co file vault.md trong project (chua credentials)
- Node.js >= 20 da cai
- Git da cai va config

## Inputs
### Required
- File GIT-DB-Structure.md — chua danh sach repos va DB connections
- File vault.md — chua git credentials, DB passwords, API keys
### Optional
- Ten module cu the can setup (neu chi setup 1 phan)
- Branch cu the can checkout (default: staging)

## Output Contract
- Tat ca repos da clone thanh cong vao may local
- DB connection verified (co the query)
- Dev server co the start (npm run dev hoac tuong duong)
- File .env da tao voi dung credentials

## Internal Reasoning Checklist
- [ ] Doc GIT-DB-Structure.md → xac dinh repos nao thuoc module nao
- [ ] Doc vault.md → lay credentials cho git va DB
- [ ] Kiem tra network access toi gitlab.clevai.vn
- [ ] Kiem tra DB port co mo khong (5432 cho Postgres, 3306 cho MySQL)
- [ ] Xac dinh branch can checkout (staging-c, main, etc.)

## 📥 Download từ SEPO (trước khi execute)

**Docs cần có để chạy step này:**

| File/Doc | Filter | Action |
|----------|--------|--------|
| `GIT-DB-Structure.md` | title chứa `GIT-DB-Structure` | Nếu local chưa có → download |
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
#    - Filter: title chứa "GIT-DB-Structure"
#    - Check local: ls **/*GIT-DB-Structure* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
#    - Filter: title chứa "vault"
#    - Check local: ls **/*vault* → đã có?
#    - Nếu thiếu: GET /api/docs/{docId}/versions → save contentText
```

## 📤 Upload lên SEPO (sau khi hoàn thành)

> Step này **không có output file cần upload** (chỉ là code commit / config change).

## Execution Procedure
1. Doc file GIT-DB-Structure.md de xac dinh TAT CA git repos phuc vu module hien tai
2. Doc file vault.md de lay credentials (git token, DB password, API keys)
3. Voi moi repo: git clone https://[token]@gitlab.clevai.vn/[group]/[repo].git
4. Checkout branch phu hop (staging hoac branch chi dinh)
5. Tao file .env tu .env.example, dien credentials tu vault.md
6. Chay npm install trong moi repo
7. Test DB connection: chay 1 query SELECT don gian
8. Thu start dev server: npm run dev
9. Verify: server start OK, khong co error

## Branching Logic
- Neu repo da ton tai → git pull thay vi git clone
- Neu DB khong ket noi duoc → kiem tra firewall, VPN, credentials
- Neu npm install fail → xoa node_modules va package-lock.json, chay lai
- Neu co nhieu modules → hoi user setup tat ca hay chi 1 module

## Validation Checklist
- [ ] Moi repo: git status khong co loi
- [ ] DB: SELECT 1 tra ve thanh cong
- [ ] Dev server: curl localhost:PORT/health tra ve 200
- [ ] .env file ton tai va co dung format

## Failure Cases
- Git clone fail: sai token hoac repo khong ton tai
- DB connection refused: sai host/port/password hoac firewall block
- npm install fail: Node version khong tuong thich
- Dev server crash on start: thieu env vars hoac DB chua san sang

## Recovery Actions
- Git fail → kiem tra vault.md co dung token, thu login gitlab bang browser
- DB fail → kiem tra host:port bang telnet/nc, xac nhan password trong vault.md
- npm fail → xoa node_modules, package-lock.json, chay npm cache clean --force
- Server fail → doc error log, kiem tra .env day du

## Completion Criteria
- TAT CA repos da clone va o dung branch
- DB connection verified
- Dev server start thanh cong
- Developer co the bat dau code

## Next-Step Handoff
- T110 (L1: Voi tong) — bat dau phan tich he thong

## Reusable Prompt Block
```text
Step 1:
Đọc file GIT-DB-Structure.md và vault.md. Sau đó:
1. Xác định tất cả git repos phục vụ module [MODULE] từ ma trận trong GIT-DB-Structure.md
2. Dùng credentials trong vault.md để clone toàn bộ repos đó về thư mục [MODULE] trên local
3. Kết nối vào database (credentials trong vault.md), liệt kê tất cả databases và các bảng trong từng database
4. Sau khi kết nối xong, liệt kê các bảng của các DB đã kết nối; và các repo đã pull
5. Sau khi connect các bảng Staging, kiểm tra xem có data chưa, nếu chưa thì lấy ít nhất 100 dòng gần nhất cho mỗi bảng từ Production về Staging
6. Kiểm tra tất cả các bảng theo các db đã connect đều có dữ liệu, nếu không thì thông báo lại.

Các MODULE cần connect database và git là:
- {MODULE}
- {MODULE}
- {MODULE}

Step 2: 
1. Đọc và bắt buộc sử dụng CLAUDE.md cho toàn bộ các prompt.
2 Trong file CLAUDE.md hỏi user các thông tin cần thiết để setup lần đầu. Các thông tin chưa có sẽ bổ sung tiếp trong quá trình làm dự án.
2. Đọc kỹ  và sử dụng các file "Company-design-system", "Clevai_Tech_Stack_Prefer" trong quá trình làm dự án
Step 3:
Download và cài đặt https://github.com/garrytan/gstack.git
```
