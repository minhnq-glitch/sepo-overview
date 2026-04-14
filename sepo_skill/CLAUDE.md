# CLAUDE.md — SEPO Skill (L1: Global)

> **Level:** L1 — Always loaded. Injected into ALL prompts.
> **Download:** Into root folder (~/.claude/skills/sepo_skill/)
> **Activate:** Always active in every Claude Code session within SEPO context.

---

## 1. SEPO Process Overview

SEPO (Software Engineering Process Orchestrator) manages the full lifecycle of building a software product:

```
T0 Setup → T1 Analysis → T2 Architecture → T3 Build → T4 Deploy & UAT → T5 Production
```

Each phase has steps (T010, T110, T212...). Each step has:
- **SKILL.md** — instructions + prompt for Claude to execute
- **CLAUDE.md** — rules & constraints Claude MUST follow

## 2. Language

| Context | Language |
|---------|----------|
| Communication with user | Vietnamese |
| Code, commits, file names | English |
| Documentation | Vietnamese (with English technical terms) |
| CLAUDE.md content | Vietnamese or English (match project) |

## 3. Three Iron Rules (apply to ALL steps)

### Rule 1: STAY IN SCOPE
- Only edit files for the CURRENT step
- If you find a bug outside scope → report it, do NOT fix it
- Do NOT refactor code that isn't part of the step

### Rule 2: REUSE
- Check existing helpers, components, utilities BEFORE creating new ones
- Search codebase with grep/glob before writing new code
- If a function exists that does 80% of what you need → extend it, don't duplicate

### Rule 3: VERIFY
- Build must pass after EVERY change: `npm run check`
- Run tests after EVERY change: `npm test` or `vitest run`
- Never commit code that doesn't build

## 4. Git Conventions

```
type(scope): description

Co-Authored-By: Claude <noreply@anthropic.com>
```

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`
Branch: `staging-c` (default development)

### Do NOT commit:
- .env files
- vault.md
- node_modules/
- API keys, passwords, tokens

## 5. SEPO Web API

Base URL: `https://sepo.mikai.tech` (staging)

```bash
# Login (dev quick-login)
curl -s -c cookies.txt -X POST \
  -H "Content-Type: application/json" \
  -d '{"email":"tuanpm@clevai.edu.vn"}' \
  ${BASE}/api/dev/quick-login

# Get projects
curl -s -b cookies.txt ${BASE}/api/projects

# Get docs for project
curl -s -b cookies.txt "${BASE}/api/docs?projectId=${PID}"

# Get doc content (versions)
curl -s -b cookies.txt ${BASE}/api/docs/${DOC_ID}/versions

# Upload doc
POST /api/docs + POST /api/docs/{id}/versions

# Get task-item status
curl -s -b cookies.txt "${BASE}/api/task-items?projectId=${PID}"
```

## 6. Task Classification (A/B/C)

Read `layerevent.md` in project docs for full rules.

| Type | Definition | Approval | Scope |
|------|-----------|----------|-------|
| **A** | UI/API only, no schema change | None | Frontend + API routes |
| **B** | Changes schema, moderate complexity | Team Lead | Backend + DB migration |
| **C** | Touches core data/event layers | Team Lead + Architect | Event system, cross-service |

## 7. Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Express 5 + TypeScript (tsx) → port 5000 |
| Frontend | React 18 + Vite + Tailwind CSS + Radix UI + wouter |
| Database | PostgreSQL (Drizzle ORM) — schema at `shared/schema.ts` |
| Auth | Passport (Google OAuth2 + Local) |
| State | TanStack React Query |
| Path aliases | `@` → client/src/, `@shared` → shared/, `@assets` → attached_assets/ |

## 8. Download & Activation Matrix

| Level | What | Download to | When to download | When to activate |
|-------|------|-------------|-----------------|-----------------|
| **L1** | This file | ~/.claude/skills/sepo_skill/ | Always — install once | Always loaded in every prompt |
| **L2** | Phase CLAUDE.md | Project folder | Lazy: when Claude detects phase context | When working in project folder |
| **L3** | Step CLAUDE.md | Project folder | When user starts step Txx | When executing step Txx |

## 9. Do / Don't

### DO:
- Always read CLAUDE.md before starting any step
- Always download required docs from SEPO before executing
- Always ask user before uploading output to SEPO
- Always verify (build + test) after making changes
- Always follow Iron Rules

### DON'T:
- Don't skip verification steps
- Don't commit without passing build
- Don't create new files when existing ones can be extended
- Don't fix out-of-scope issues
- Don't hardcode credentials
- Don't push directly to main/prod without going through staging
