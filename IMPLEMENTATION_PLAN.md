# 🏃‍♂️ 디투런 출석 관리 시스템 - 구현 계획서

## 📋 프로젝트 개요
- **목적**: 크루원 출석 기록 관리 및 운영 규칙 자동화
- **주요 기능**: 출석 관리, 일정 관리, 규칙 자동화, 통계/분석
- **기술 스택**: (기획서 기반 - 추후 결정)

---

## 🚀 구현 페이즈별 계획

### **Phase 1: 프로젝트 설정 & 데이터베이스 설계**
- [x] 프로젝트 초기 설정 (개발 환경 구성)
  - [x] Docker Compose (PostgreSQL + FastAPI + Next.js)
  - [x] .gitignore, README, 프로젝트 구조
- [x] 기술 스택 선정 (Backend, Frontend, DB)
  - [x] Backend: Python 3.12 + FastAPI 0.111+
  - [x] Frontend: Next.js 14 (TypeScript) + Tailwind CSS
  - [x] DB: PostgreSQL 16 + SQLAlchemy 2.0 + Alembic
  - [x] ORM: SQLAlchemy async + asyncpg
- [x] 데이터베이스 스키마 설계
  - [x] Member (크루원) 테이블
  - [x] Schedule (일정) 테이블
  - [x] Attendance (출석) 테이블
  - [x] RuleApplicationLog (규칙 로그) 테이블
- [x] API 문서 작성 (OpenAPI/Swagger)
  - [x] OpenAPI 3.1 spec (16 엔드포인트)
- [x] ORM 관계 매핑 (Phase 2 준비)
  - [x] Member.attendances, Member.created_schedules
  - [x] Schedule.attendances, Schedule.creator
  - [x] Attendance.member, Attendance.schedule
- [x] 테스트 기초 구축
  - [x] pytest + pytest-asyncio 설정
  - [x] 헬스체크 엔드포인트 테스트
- [x] 샘플 데이터 시드 스크립트
  - [x] CSV 기반 크루원 자동 import (23명)

**예상 기간**: 1주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-28

---

### **Phase 2: 기본 데이터 모델 & REST API 구축**
- [x] Member 모델 및 CRUD API 구현
- [x] Schedule 모델 및 CRUD API 구현
- [x] Attendance 모델 및 CRUD API 구현
- [x] 인증/인가 시스템 구축 (JWT + admin 계정)
- [x] 에러 처리 및 로깅 시스템 (커스텀 예외, JSON 로거)
- [x] API 테스트 작성 (test_members, test_schedules, test_attendance, test_auth)

**예상 기간**: 2주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-28

---

### **Phase 3: 출석 관리 기능**
- [x] 일정별 출석 체크 API
  - [x] 수동 체크 기능 (method=manual)
  - [x] 출석 상태 관리 (attended/no_show)
- [x] 출석 기록 조회 API (schedule_id/member_id/status 필터)
- [x] 출석 수정/삭제 기능
- [x] 단위 테스트 작성 (test_attendance.py)

**예상 기간**: 1주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-28

---

### **Phase 4: 일정 관리 기능**
- [x] 정기런 일정 관리 (월/목 자동 생성)
  - [x] FastAPI lifespan + asyncio 배경 루프 (APScheduler 불필요)
  - [x] get_mon_thu_dates_in_range, generate_regular_schedules, run_scheduler_loop
- [x] 비정기런 일정 생성/수정/삭제
- [x] 대회/비러닝 일정 관리
- [x] 일정 목록 조회 및 필터링 (type 필터)
- [x] 일정 상세 정보 API
- [x] 통합 테스트 작성 (test_schedules.py, test_scheduler.py)

**예상 기간**: 1주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-29

---

### **Phase 5: 규칙 자동화 시스템**
- [x] 일정 개설 조건 검증
  - [x] 최근 6개월 내 러닝 일정 3회 이상 참석 확인
- [x] 일정 참여 제한 규칙
  - [x] 러닝 일정: 누구나 참여 가능
  - [x] 대회/비러닝: 러닝 3회 이상 참석자만 참여
- [x] 강퇴 조건 자동 판별
  - [x] 노쇼 2회 이상 감지 및 상태 변경 (check_and_apply_no_show_removal)
- [x] 규칙 자동 실행 (출석 기록 시 자동 트리거)
- [x] 규칙 적용 로그 기록 (RuleApplicationLog)
- [x] 규칙 검증 테스트 작성 (test_rule_engine.py - 13개)

**예상 기간**: 2주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-29

---

### **Phase 6: 통계 & 분석 기능**
- [x] 개인 출석 통계 API (GET /api/v1/stats/members/{id})
  - [x] 출석 횟수
  - [x] 6개월 러닝 참석 횟수
- [x] 기간별 통계
  - [x] 월별 통계 (attendance_by_month)
- [x] 출석 랭킹 API (GET /api/v1/stats/members - running_attended_6m 정렬)
- [x] 대시보드 요약 API (GET /api/v1/stats/summary)
- [ ] 캐싱 및 성능 최적화 (추후)
- [x] 통계 관련 테스트 작성 (test_stats.py - 10개)

**예상 기간**: 1.5주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 80% (기본 통계 + 테스트 완료 / 캐싱 미구현)
**완료일**: 2026-04-29 (기본 구현)

---

### **Phase 7: QR 코드 기반 출석 시스템**
- [x] QR 코드 생성 기능 (POST /api/v1/qr/schedules/{id}/generate)
- [x] QR 코드 검증 API (POST /api/v1/qr/verify)
- [x] QR 스캔 출석 처리 (method=qr, 자동 attended 기록)
- [x] QR 코드 관리 (갱신 /refresh, 폐기 DELETE)
- [x] 모바일 QR 스캔 UI 개발 (/qr 관리자 페이지, /check-in 크루원 페이지)
- [x] QR 시스템 보안 및 무결성 검증 (토큰 만료, 비활성화, 중복 체크)
- [x] QR 시스템 테스트 (test_qr.py - 14개)

**예상 기간**: 1주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-29

---

### **Phase 8: 관리자 대시보드 UI**
- [x] 대시보드 메인 페이지 (통계 요약) → /dashboard
- [x] 크루원 관리 (추가/조회)
- [x] 일정 관리 (추가/목록)
- [x] 출석 관리 페이지 (출석표 - 체크/노쇼/취소)
- [x] 규칙 설정 페이지 → /rules (읽기 전용 규칙 표시)
- [x] 통계/분석 페이지 → /stats
- [x] Next.js 14 + Tailwind CSS 기반 UI 구현

**예상 기간**: 2주
**담당자**: Claude + eommji@gmail.com
**완료 상태**: 100% ✅
**완료일**: 2026-04-29

---

### **Phase 9: 모바일 앱 UI (크루원용)**
- [ ] 출석 현황 페이지
- [ ] 참여 일정 목록
- [ ] QR 스캔 페이지
- [ ] 개인 통계 페이지
- [ ] 알림 기능
- [ ] 반응형 디자인 적용

**예상 기간**: 2주
**담당자**: (미정)
**완료 상태**: 0%

---

### **Phase 10: 배포 & 운영 준비**
- [ ] 개발 환경 배포
- [ ] 스테이징 환경 구축
- [ ] 프로덕션 환경 구축
- [ ] CI/CD 파이프라인 구성
- [ ] 모니터링 & 로깅 시스템 구축
- [ ] 백업 & 재해 복구 계획
- [ ] 운영 문서 작성

**예상 기간**: 1주
**담당자**: (미정)
**완료 상태**: 0%

---

### **Phase 11: 통합 테스트 & QA**
- [ ] 전체 기능 통합 테스트
- [ ] 성능 테스트
- [ ] 보안 취약점 점검
- [ ] 사용자 수용 테스트 (UAT)
- [ ] 버그 수정 및 최적화

**예상 기간**: 1.5주
**담당자**: (미정)
**완료 상태**: 0%

---

### **Phase 12: 런칭 & 운영**
- [ ] 본 운영 배포
- [ ] 사용자 교육 및 온보딩
- [ ] 운영 지원 체계 구축
- [ ] 초기 피드백 수집
- [ ] 안정성 모니터링

**예상 기간**: 지속적
**담당자**: (미정)
**완료 상태**: 0%

---

## 📊 프로젝트 일정 요약

| 페이즈 | 주요 목표 | 예상 기간 | 상태 |
|--------|---------|---------|------|
| 1 | 프로젝트 설정 & DB 설계 | 1주 | ✅ |
| 2 | 기본 API 구축 | 2주 | ✅ |
| 3 | 출석 관리 | 1주 | ✅ |
| 4 | 일정 관리 | 1주 | ✅ |
| 5 | 규칙 자동화 | 2주 | ✅ |
| 6 | 통계/분석 | 1.5주 | 🔶 |
| 7 | QR 코드 시스템 | 1주 | ✅ |
| 8 | 관리자 대시보드 | 2주 | ✅ |
| 9 | 모바일 앱 | 2주 | ⏳ |
| 10 | 배포 준비 | 1주 | ⏳ |
| 11 | 통합 테스트 | 1.5주 | ⏳ |
| 12 | 런칭 & 운영 | 지속적 | ⏳ |
| **총합** | **전체 프로젝트** | **약 18-19주** | - |

---

## 🔄 마일스톤

- **Milestone 1** (5주): API 기본 구축 완료 (Phase 1-4)
- **Milestone 2** (7주): 핵심 기능 완료 (Phase 1-6)
- **Milestone 3** (9주): QR 시스템 완료 (Phase 1-7)
- **Milestone 4** (13주): UI/UX 완료 (Phase 1-9)
- **Milestone 5** (19주): 런칭 준비 완료 (Phase 1-11)

---

## 📝 주의사항

1. **기술 스택 결정**: Phase 1에서 Backend (Node.js/Python/etc), Frontend (React/Vue/etc), Database (PostgreSQL/MongoDB/etc) 선택 필요
2. **팀 규모**: 현재 담당자가 미정이므로, 팀 구성에 따라 일정 조정 필요
3. **API 우선 개발**: Backend API를 먼저 안정화한 후 Frontend 개발
4. **테스트 자동화**: 각 Phase마다 유닛/통합 테스트 작성
5. **규칙 자동화 검증**: Phase 5는 복잡도가 높으므로 충분한 테스트 시간 필요

---

## ✅ 체크리스트 사용 방법

각 항목의 `[ ]`를 `[x]`로 변경하여 완료 상태를 표시합니다.

**예시**:
```
- [x] 프로젝트 초기 설정
- [ ] 기술 스택 선정
```

---

**수정 이력**
- v1.5: Phase 7 완료 / Phase 6 테스트 추가 (2026-04-29) - QR 코드 생성·검증·갱신·폐기 API, QR 관리자 UI(/qr), 크루원 체크인 페이지(/check-in), test_stats.py(10개) + test_qr.py(14개) 추가, 전체 86개 테스트 통과
- v1.4: Phase 4·5·8 완료 / Phase 6 기본 완료 (2026-04-29) - 정기런 자동 생성 스케줄러, 규칙 자동화 엔진 (생성 자격·참여 제한·노쇼 강퇴), 통계 API, 대시보드·통계·규칙 UI 페이지
- v1.3: Phase 3 완료 / Phase 4·8 부분 완료 (2026-04-28) - 출석 관리 UI (Next.js), SQLite 개발 환경, limit 버그 수정
- v1.2: Phase 2 완료 (2026-04-28) - Member/Schedule/Attendance CRUD API, JWT 인증, 에러 처리, 테스트 작성
- v1.1: Phase 1 완료 (2026-04-28) - 프로젝트 설정, DB 스키마, ORM 관계, 테스트, 시드 스크립트 완성
- v1.0: 초안 작성 (2026-04-28)

