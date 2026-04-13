# CLAUDE.md — Task T010: Setup ClaudeCode

## Task Identity
- **Code:** T010
- **Name:** Setup ClaudeCode: Pull Git va ket noi DB
- **Phase:** T0 — Setup Environment
- **Classification:** Type A (UI/API only, no schema change)
- **Expert Team:** DevOps Engineer + Backend Developer

## Objective
Thiet lap moi truong phat trien: clone tat ca git repos lien quan va ket noi database de developer co the bat dau code ngay.

## Preconditions
- Co quyen truy cap GitLab (gitlab.clevai.vn)
- Node.js >= 20 da cai
- Git da cai va config

## Input Docs (Required)
- **GIT-DB-Structure.md** — Danh sach repos va DB connections
- **vault.md** — Git credentials, DB passwords, API keys

## Output Contract
- Tat ca repos da clone thanh cong vao may local
- DB connection verified (co the query)
- Dev server co the start (`npm run dev`)
- File `.env` da tao voi dung credentials

## Instructions

### Step 1: Clone Repos & Connect DB
1. Doc file GIT-DB-Structure.md va vault.md
2. Xac dinh tat ca git repos phuc vu module tu ma tran trong GIT-DB-Structure.md
3. Dung credentials trong vault.md de clone toan bo repos ve thu muc local
4. Ket noi vao database (credentials trong vault.md), liet ke tat ca databases va cac bang
5. Sau khi ket noi xong, liet ke cac bang cua cac DB da ket noi va cac repo da pull
6. Sau khi connect cac bang Staging, kiem tra xem co data chua, neu chua thi lay it nhat 100 dong gan nhat cho moi bang tu Production ve Staging
7. Kiem tra tat ca cac bang theo cac db da connect deu co du lieu, neu khong thi thong bao lai

### Step 2: Setup CLAUDE.md & Standards
1. Doc va bat buoc su dung CLAUDE.md cho toan bo cac prompt
2. Trong file CLAUDE.md hoi user cac thong tin can thiet de setup lan dau
3. Doc ky va su dung cac file "Company-design-system", "Clevai_Tech_Stack_Prefer" trong qua trinh lam du an

### Step 3: Install Development Tools
Download va cai dat https://github.com/garrytan/gstack.git

## Validation Checklist
- [ ] All repos cloned successfully
- [ ] DB connections verified (can query)
- [ ] Dev server starts without errors
- [ ] `.env` file created with correct credentials
- [ ] CLAUDE.md read and understood

## Next Step
T110 — L1: Voi tong (Entity Overview)

## References
- @docs/GIT-DB-Structure.md — load for repo and DB connection info
- @docs/vault.md — load for credentials
- @docs/Company-design-system.md — load when making UI decisions
- @docs/Clevai_Tech_Stack_Prefer.md — load when choosing libraries or tools
