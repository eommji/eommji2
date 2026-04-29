# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**D2R Attendance Management System** — A system to manage running crew member attendance, automate operational rules, and provide statistics/analytics.

**Status**: 구현 진행 중. Phase 1–3 완료, Phase 4·8 부분 완료.

## Core Features

1. **Attendance Management**: Track attendance (present/no-show) for each schedule event, with manual check-in and QR code scanning support
2. **Schedule Management**: Regular runs (Monday/Thursday), irregular runs, competitions, and non-running events
3. **Rule Automation**: 
   - Event creation eligibility: Must have 3+ running event attendance in past 6 months
   - Event participation restrictions: Running events are open to all; competitions/non-running events require 3+ running attendance
   - Auto-removal: Members with 2+ no-shows are automatically removed
4. **Statistics & Analytics**: Individual attendance counts, attendance rates, period-based stats (monthly/quarterly/yearly), and attendance rankings
5. **QR Code System**: QR code generation and validation for quick attendance check-in
6. **Admin Dashboard & Mobile App UI**: Dashboards for admins and attendance/statistics pages for members

## Data Model

All data is JSON-based. Key entities:

### Member
- `id` (UUID)
- `name`, `nickname`
- `status` (active/restricted/removed)
- `joined_at` (ISO date)

### Schedule
- `id` (UUID)
- `type` (running/competition/non_running)
- `date`, `time`, `location`
- `max_participants`
- `created_by` (member_id)

### Attendance
- `id` (UUID)
- `schedule_id`, `member_id`
- `status` (attended/no_show)
- `checked_in_at` (timestamp)
- `method` (manual/qr)

### Rule Engine
Tracks:
- Eligibility rules (running event attendance thresholds)
- Participation restrictions
- Auto-removal conditions (no-show count)

Operates on 6-month rolling windows for all time-based calculations.

## Project Phases

The project follows a 12-phase implementation plan (see IMPLEMENTATION_PLAN.md):

1. **Phase 1** (1 week): Project setup & database schema design
2. **Phase 2** (2 weeks): Core data models & REST APIs
3. **Phase 3** (1 week): Attendance management APIs
4. **Phase 4** (1 week): Schedule management
5. **Phase 5** (2 weeks): Rule automation engine
6. **Phase 6** (1.5 weeks): Statistics & analytics
7. **Phase 7** (1 week): QR code system
8. **Phase 8** (2 weeks): Admin dashboard UI
9. **Phase 9** (2 weeks): Member mobile app UI
10. **Phase 10** (1 week): Deployment infrastructure
11. **Phase 11** (1.5 weeks): Integration testing & QA
12. **Phase 12** (Ongoing): Launch & operations

Total estimated time: ~18-19 weeks

## Key Business Rules

- **6-month window**: All attendance-based rules use a rolling 6-month window from the current date
- **Running event threshold**: 3+ running event attendance required for:
  - Creating/hosting events
  - Participating in competitions and non-running events
- **No-show policy**: 2 or more no-shows trigger automatic member removal from the crew
- **Regular schedule**: Monday and Thursday are automatic regular run days (should be auto-generated)

## Tech Stack (확정)

Phase 1에서 확정된 기술 스택:

| 영역 | 기술 |
|------|------|
| **Backend** | Python 3.12 + FastAPI 0.111+ |
| **Frontend** | Next.js 14 (TypeScript) + Tailwind CSS |
| **Database** | SQLite (개발) / PostgreSQL 16 (운영) |
| **ORM** | SQLAlchemy 2.0 async + asyncpg |
| **마이그레이션** | Alembic |
| **인증** | JWT |
| **테스트** | pytest + pytest-asyncio |
| **배포** | Docker Compose |
| **API 스타일** | RESTful |

## Project Structure

```
d2r-attendance/
├── backend/
│   ├── app/
│   │   ├── models/        # SQLAlchemy ORM (member, schedule, attendance, rule_log)
│   │   ├── routers/       # FastAPI 라우터 (members, schedules, attendance, auth)
│   │   ├── schemas/       # Pydantic 스키마
│   │   ├── auth.py        # JWT 인증
│   │   ├── database.py    # DB 연결 (async SQLAlchemy)
│   │   └── main.py
│   ├── alembic/           # DB 마이그레이션
│   ├── scripts/           # 시드 스크립트 (seed_members.py)
│   └── tests/             # pytest 테스트
└── frontend/
    ├── app/               # Next.js App Router
    │   └── page.tsx       # 메인 대시보드 (출석표)
    └── lib/
        └── api.ts         # API 클라이언트 래퍼
```

## Development Approach

- **API-first**: Build and stabilize backend APIs before frontend
- **Test automation**: Unit tests, integration tests, and end-to-end tests for each phase
- **Rule engine testing**: Phase 5 (rule automation) requires extensive testing due to complexity
- **Modular design**: Each phase builds on previous phases; maintain clear separation of concerns

## Important Notes

- Rule automation logic (Phase 5) is complex and time-sensitive — requires careful testing around the 6-month rolling window calculations
- The system must handle concurrent attendance operations (multiple members checking in for the same event simultaneously)
- Member status transitions (active → restricted → removed) should be logged and auditable
- Consider timezone awareness for date/time operations, especially the 6-month window calculations

## Current Artifacts

- `d2r-attendance-system.md`: 기획서 — 기능 요구사항, 데이터 구조, 기술 스택, 구현 현황
- `IMPLEMENTATION_PLAN.md`: 12페이즈 구현 계획서 (상세 태스크 및 완료 상태)
- `d2r-attendance/`: 실제 구현 코드 (backend + frontend)
- `members.csv`: 샘플 크루원 데이터 (23명)

## Development Commands

```bash
# 전체 스택 실행 (Docker)
cd d2r-attendance
docker-compose up

# 백엔드만 개발 실행 (SQLite, Docker 없이)
cd d2r-attendance/backend
DATABASE_URL=sqlite+aiosqlite:///./d2r_dev.db uvicorn app.main:app --reload

# 프론트엔드 개발 실행
cd d2r-attendance/frontend
npm run dev

# 테스트 실행
cd d2r-attendance/backend
pytest -v                          # 전체
pytest tests/test_members.py -v   # 특정 파일

# DB 마이그레이션
cd d2r-attendance/backend
alembic upgrade head               # 최신으로
alembic revision --autogenerate -m "설명"  # 새 마이그레이션 생성

# 시드 데이터 (크루원 23명)
python scripts/seed_members.py

# 코드 품질 (ruff — dev 의존성)
ruff format app/                   # 포매팅
ruff check --fix app/              # 린팅 자동 수정

# TypeScript 타입 체크
cd d2r-attendance/frontend
npx tsc --noEmit
```

## Configured Automation (.claude/settings.local.json)

- **PostToolUse hook**: `.py` 파일 편집 시 ruff format + ruff check --fix 자동 실행
- **Stop hook**: 작업 완료 시 알림 표시
- **Permissions**: pytest, ruff, poetry, alembic, docker-compose, npm, npx tsc/eslint 자동 허용
- **MCP**: Supabase (`agljjkqixldylkfkpabi` 프로젝트) 연결됨
