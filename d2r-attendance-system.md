# 🏃‍♂️ 디투런 출석 관리 시스템 기획서 (v1)

## 1. 🎯 목적
- 크루원 출석 기록 관리
- 운영 규칙 자동 판별
- 활동성 기반 크루 운영 (참여 / 제한 / 강퇴)

---

## 2. 🧱 핵심 기능

### 2.1 출석 관리
- 일정별 출석 체크
- 상태: **참석 / 노쇼**
- 운영진 수동 체크 + QR 체크 지원

---

### 2.2 일정 관리
- 정기런 (월/목 고정)
- 비정기런
- 대회 / 비러닝 일정 포함

---

### 2.3 규칙 자동화

#### ✅ 일정 개설 조건
- 최근 6개월 내 **러닝 일정 3회 이상 참석자**

#### ✅ 일정 참여 제한
- 러닝 일정 → 누구나 가능  
- 대회 / 비러닝 → 최근 6개월 내 **러닝 3회 이상 참석자**

#### ❌ 강퇴 조건
- **노쇼 2회 이상**

---

### 2.4 통계 / 분석
- 개인 출석 횟수 / 출석률
- 기간별 (월 / 3개월 / 연간)
- 출석 랭킹

---

## 3. 🗂 데이터 구조 설계

### 3.1 Member (크루원)

```json
{
  "id": "uuid",
  "name": "홍길동",
  "nickname": "길동",
  "status": "active",
  "joined_at": "2026-04-01",
  "created_at": "2026-04-01T00:00:00Z",
  "updated_at": "2026-04-01T00:00:00Z"
}
```

`status` 값: `active` / `restricted` / `removed`

---

### 3.2 Schedule (일정)

```json
{
  "id": "uuid",
  "type": "running",
  "date": "2026-05-05",
  "time": "07:00:00",
  "location": "한강공원 여의도지구",
  "max_participants": null,
  "created_by": "member-uuid",
  "created_at": "2026-04-01T00:00:00Z"
}
```

`type` 값: `running` / `competition` / `non_running`

---

### 3.3 Attendance (출석)

```json
{
  "id": "uuid",
  "schedule_id": "schedule-uuid",
  "member_id": "member-uuid",
  "status": "attended",
  "checked_in_at": "2026-05-05T07:10:00Z",
  "method": "manual",
  "created_at": "2026-05-05T07:10:00Z"
}
```

`status` 값: `attended` / `no_show`  
`method` 값: `manual` / `qr`

---

### 3.4 RuleApplicationLog (규칙 적용 로그)

```json
{
  "id": "uuid",
  "member_id": "member-uuid",
  "action": "status_change_to_removed",
  "reason": "노쇼 2회 초과",
  "triggered_by": "admin-uuid",
  "snapshot": { "no_show_count": 2 },
  "created_at": "2026-05-05T07:10:00Z"
}
```

`action` 값: `status_change_to_restricted` / `status_change_to_removed` / `participation_blocked` / `creation_blocked`

---

## 4. 🛠 기술 스택 (확정)

| 영역 | 기술 |
|------|------|
| Backend | Python 3.12 + FastAPI 0.111+ |
| Frontend | Next.js 14 (TypeScript) + Tailwind CSS |
| Database | SQLite (개발) / PostgreSQL 16 (운영) |
| ORM | SQLAlchemy 2.0 async + asyncpg |
| 마이그레이션 | Alembic |
| 인증 | JWT |
| 테스트 | pytest + pytest-asyncio |
| 배포 | Docker Compose |
| API 스타일 | RESTful |

---

## 5. 📋 구현 현황

| 페이즈 | 내용 | 상태 |
|--------|------|------|
| 1 | 프로젝트 설정 & DB 설계 | ✅ 완료 |
| 2 | 기본 API (CRUD + 인증) | ✅ 완료 |
| 3 | 출석 관리 API | ✅ 완료 |
| 4 | 일정 관리 (정기런 자동 생성 제외) | 🔶 80% |
| 5 | 규칙 자동화 | ⏳ 미시작 |
| 6 | 통계/분석 | ⏳ 미시작 |
| 7 | QR 코드 | ⏳ 미시작 |
| 8 | 관리자 대시보드 UI | 🔶 55% |
| 9–12 | 모바일 / 배포 / QA / 런칭 | ⏳ 미시작 |

상세 구현 계획: `IMPLEMENTATION_PLAN.md` 참조
