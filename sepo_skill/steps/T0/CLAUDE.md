# CLAUDE.md — Phase T0: Setup (L2: Phase)

> **Level:** L2 — Lazy loaded when user enters Setup phase.
> **Download:** Into project folder.
> **Activate:** When Claude detects setup context (clone, install, .env, DB connect).

---

## Scope
Set up development environment: clone all git repos, connect database, configure environment variables, verify dev server starts.

## Steps in this phase
| Step | Name | Expert Team |
|------|------|-------------|
| T010 | Setup ClaudeCode: Pull Git & Connect DB | DevOps + Backend Dev |

## Prerequisites
- Node.js >= 20 installed
- Git installed and configured
- Network access to gitlab.clevai.vn
- vault.md available (contains credentials)

## Key Files
- `GIT-DB-Structure.md` — lists all repos + DB connections per module
- `vault.md` — credentials (git tokens, DB passwords, API keys)
- `.env.example` → `.env` — environment configuration template

## Rules
1. ALWAYS read GIT-DB-Structure.md FIRST to identify repos
2. ALWAYS read vault.md for credentials
3. NEVER hardcode credentials — always use .env
4. NEVER commit .env or vault.md
5. Verify DB connection with `SELECT 1` before proceeding
6. Verify dev server starts with `npm run dev`

## .env Template
```
DATABASE_URL=postgresql://user:pass@host:5432/dbname
SESSION_SECRET=random_32_chars_minimum
PORT=5000
NODE_ENV=development
SKIP_SEED=true
```

## Success Criteria
- [ ] All repos cloned to local machine
- [ ] Correct branch checked out (staging-c)
- [ ] .env created with valid credentials
- [ ] DB connection verified
- [ ] Dev server starts without errors
- [ ] Health check returns 200

## After this phase → T1 (Ve Voi: System Analysis)
