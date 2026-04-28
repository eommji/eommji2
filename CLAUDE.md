# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**D2R Attendance Management System** — A system to manage running crew member attendance, automate operational rules, and provide statistics/analytics.

**Status**: Early planning phase. Architecture and tech stack not yet finalized.

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

## Architecture Decisions Pending

The following decisions remain unspecified and should be made during Phase 1:

- **Backend Framework**: Node.js, Python, Java, Go, etc.
- **Frontend Framework**: React, Vue, Next.js, etc.
- **Database**: PostgreSQL, MongoDB, MySQL, etc.
- **API Style**: RESTful, GraphQL, or hybrid
- **Authentication**: JWT, OAuth2, sessions, etc.
- **Deployment**: Docker, cloud platforms, on-premises

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

- `d2r-attendance-system.md`: Original specification document with detailed requirements
- `IMPLEMENTATION_PLAN.md`: 12-phase implementation roadmap with detailed task breakdowns
- `members.csv`: Sample member data for reference (23 crew members with various attendance records)
