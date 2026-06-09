# OfficeOps Hub

## 문서 버전 이력

| 버전 | 기준 | 수정 사항 | 삭제 사항 |
| --- | --- | --- | --- |
| v1.0.0 | 관리자 전용 계정 생성 반영 이전 기준 문서 | 기존 프로젝트 개요, 기능 범위, 기술 스택, 팀 프로젝트 안내 기준선 | 없음 |
| v1.1.0 | 관리자 전용 계정 생성 반영 | 인증 기능을 관리자 사용자 계정 생성과 로그인 중심으로 정리, 권한 설명에 HR/재무/시스템 관리자 역할 반영, API 요약을 `POST /api/admin/users` 기준으로 수정 | 공개 회원가입 중심 설명과 `POST /api/auth/signup` 요약 제거 |
| v1.2.0 | 문서 네이밍 및 버전 관리 체계 정리 | 문서 파일명을 번호 없는 한글 제목 기반 규칙으로 정리하고 버전 표기를 semantic version 형식으로 통일 | 문서 번호 접두어와 영문 기반 산출물 파일명 제거 |
| v1.3.0 | 파일명 버전 최신화 규칙 반영 | 문서 파일명의 버전을 문서 내부 최신 버전과 동일하게 관리하도록 정리하고, 이후 수정 및 버전 상승 시 파일명과 참조 링크를 즉시 갱신하는 규칙 추가 | 최신 버전과 맞지 않는 파일명 버전 표기 제거 |
| v1.4.0 | 문서별 독립 버전 관리 정책 반영 | 문서마다 실제 업데이트 여부를 기준으로 버전을 상승시키고, 수정된 문서의 파일명과 참조 링크만 함께 갱신하도록 규칙 정리 | 전체 문서 버전을 일괄로 맞춘다는 해석 제거 |
| v1.5.0 | README 작성 가이드 명칭 반영 | 문서 목록의 README 작성 가이드 파일명과 링크를 `OfficeOps_hub_README작성가이드_v1.5.0.md` 기준으로 수정 | `읽어보기작성가이드` 파일명 참조 제거 |
| v1.6.0 | 가이드/템플릿 산출물 정리 | 문서 목록에서 README 작성 가이드와 팀 프로젝트 기획안 템플릿 항목 제거 | `OfficeOps_hub_README작성가이드_v1.5.0.md`, `OfficeOps_hub_팀프로젝트기획서템플릿_v1.3.0.md` 산출물 제거 |
| v1.7.0 | 로컬/원격 브랜치 생성 지침 반영 | 협업 방식 설명과 팀 운영 문서 링크를 `dev` 기준 브랜치/PR 규칙에 맞게 수정 | `develop` 기반 브랜치 작업 표현 제거 |
| v1.8.0 | 원격 업로드 요청 처리 지침 반영 | 팀 운영 문서 링크를 원격 업로드 요청 처리 규칙이 포함된 버전으로 갱신 | 없음 |
| v1.9.0 | 보조 도구 작업 흔적 비노출 지침 반영 | 프로젝트 개요서와 팀 운영 문서 링크를 보조 도구 작업 흔적 비노출 규칙이 반영된 버전으로 갱신 | 없음 |
| v1.10.0 | 브랜치 생성 절차 보강 반영 | 프로젝트 개요서와 팀 운영 문서 링크를 브랜치 생성 절차가 보강된 버전으로 갱신 | 없음 |

사내 비품 요청, 방문 신청, 시설 요청, 회의실 예약, 자산 대여/반납 업무를 통합 관리하는 팀 프로젝트 기반 사내 운영 관리 웹 애플리케이션입니다.

## Overview

`OfficeOps Hub`는 회사 내부에서 반복적으로 발생하는 운영 요청과 예약, 자산 관리 업무를 하나의 시스템에서 처리하기 위한 프로젝트입니다.

일반 직원은 요청과 예약을 등록하고 진행 상태를 확인할 수 있습니다. 팀장은 소속 팀원의 요청을 1차 승인하거나 반려하고, 운영 담당자는 최종 승인 후 담당자와 마감일을 지정해 요청을 처리합니다. 관리자는 전체 요청, 예약, 자산, 사용자, 감사 이력을 관리하고 대시보드에서 운영 현황을 확인합니다.

이 프로젝트는 단순 CRUD를 넘어 권한 분리, 요청 상태 전이, 예약 중복 방지, 자산 상태 관리, 감사 이력 저장, 검색/필터/페이지네이션, 대시보드를 포함하는 실무형 팀 프로젝트를 목표로 합니다.

## Team Project

이 저장소는 2인 팀 프로젝트를 기준으로 설계되었습니다.

| 역할 | 주요 담당 |
| --- | --- |
| Backend | 인증/인가, 요청 API, 상태 전이, DB 설계, 이력 저장, 테스트 |
| Frontend | React 화면, 라우팅 보호, 요청/예약/자산 화면, 관리자 대시보드 |
| Common | 요구사항 정리, ERD, API 명세, GitHub Issues, README, 발표자료, 배포 |

권장 협업 방식은 기능 단위 이슈 생성, `dev` 하위 브랜치 작업, `dev` 대상 PR 리뷰, 문서 선반영입니다.

## Features

- 관리자 사용자 계정 생성, 로그인, JWT 기반 인증
- 일반 직원, 팀장, 운영 담당자, HR 담당자, 재무 담당자, 시스템 관리자 권한 분리
- 비품 요청, 방문 신청, 시설 요청 등록
- 팀장 1차 승인/반려
- 운영 담당자 최종 승인/반려
- 담당자 지정, 처리 예정일, 마감일 관리
- 처리 중, 완료, 취소 상태 변경
- 요청 상태 변경 이력과 승인 이력 저장
- 회의실/자원 예약과 중복 예약 방지
- 자산 등록, 수정, 상태 변경, 대여/반납
- 관리자 대시보드
- 감사 이력 조회
- 검색, 필터, 페이지네이션
- Swagger/OpenAPI 기반 API 문서화

## Tech Stack

### Backend

| Category | Stack |
| --- | --- |
| Language | Java 21 |
| Framework | Spring Boot 3 |
| Security | Spring Security 6, JWT |
| ORM | Spring Data JPA |
| Database | PostgreSQL |
| Migration | Flyway |
| Validation | Bean Validation |
| API Docs | Swagger/OpenAPI |
| Test | JUnit 5, Mockito |

### Frontend

| Category | Stack |
| --- | --- |
| Library | React |
| Build Tool | Vite |
| Routing | React Router |
| API Client | Axios |
| Server State | TanStack Query |
| Client State | Zustand |
| Styling | Tailwind CSS |
| Form | React Hook Form, Zod |
| Chart | Chart.js or ApexCharts |

### Infra

| Category | Stack |
| --- | --- |
| Local Environment | Docker Compose |
| Deploy | AWS EC2, Nginx |
| Database | AWS RDS PostgreSQL |
| CI/CD | GitHub Actions |

## Core Workflow

```text
사용자 요청 등록
-> 팀장 1차 승인 또는 반려
-> 운영 담당자 최종 승인 또는 반려
-> 담당자 지정 및 마감일 설정
-> 처리 중
-> 완료
-> 상태 변경 이력과 감사 이력 저장
```

## Project Structure

현재 저장소는 문서 산출물 중심으로 구성되어 있으며, 구현 단계에서 `backend`, `frontend`, `docker-compose.yml`을 추가하는 구조를 권장합니다.

```text
OfficeOps_Hub
 ├─ backend
 │   ├─ src
 │   ├─ build.gradle
 │   └─ Dockerfile
 ├─ frontend
 │   ├─ src
 │   ├─ package.json
 │   └─ Dockerfile
 ├─ docs
 │   ├─ project_charter
 │   ├─ team_charter
 │   ├─ specifications
 │   ├─ guides
 │   ├─ templates
 │   └─ prototypes
 ├─ docker-compose.yml
 └─ README.md
```

## Documentation

| Document | Description |
| --- | --- |
| [Project Charter](./docs/project_charter/OfficeOps_hub_프로젝트개요서_v1.8.0.md) | 목표, 범위, 역할, 일정, 산출물을 포함한 공식 프로젝트 개요서 |
| [Detailed Plan](./docs/project_charter/OfficeOps_hub_상세계획서_v1.3.0.md) | 상세 기능 범위와 설계 방향 |
| [Team Charter](./docs/team_charter/OfficeOps_hub_팀운영문서_v1.8.0.md) | 개발 환경, 팀 협업 방식, Git/PR 규칙 |
| [요구사항 정의서](./docs/specifications/OfficeOps_hub_요구사항정의서_v1.3.0.md) | 요구사항, MVP 범위, 성공 기준 |
| [기능 명세서](./docs/specifications/OfficeOps_hub_기능명세서_v1.3.0.md) | 기능별 처리 규칙과 예외 조건 |
| [화면 정의서](./docs/specifications/OfficeOps_hub_화면정의서_v1.3.0.md) | React 화면, URL, 권한, UI 정책 |
| [ERD/DB 설계서](./docs/specifications/OfficeOps_hub_데이터베이스설계서_v1.3.0.md) | 테이블, 관계, 인덱스, 마이그레이션 순서 |
| [API 명세서](./docs/specifications/OfficeOps_hub_인터페이스명세서_v1.3.0.md) | REST API, 요청/응답, 에러 코드 |
| [상태/권한 정책](./docs/specifications/OfficeOps_hub_상태권한정책문서_v1.3.0.md) | 상태 전이, 권한, 데이터 접근 규칙 |
| [테스트 계획서](./docs/specifications/OfficeOps_hub_테스트계획서_v1.3.0.md) | 필수 테스트와 QA 시나리오 |
| [화면 흐름도](./docs/specifications/OfficeOps_hub_화면흐름도_v1.3.0.md) | Fluid UI 기준 화면 흐름 |
| [UX Prototype](./docs/prototypes/OfficeOps_hub_사용자경험흐름프로토타입_v1.3.0.html) | 화면 흐름 확인용 HTML 프로토타입 |

## Requirements

구현 단계 기준 권장 환경입니다.

- JDK 21
- Node.js 20 LTS 이상
- npm
- Docker Desktop
- PostgreSQL
- Git

## Quick Start

현재 저장소는 기획 및 설계 문서가 먼저 정리된 상태입니다. 구현 단계에서 아래 구조와 명령어를 기준으로 실행합니다.

### Backend

```bash
cd backend
./gradlew bootRun
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

### Database

```bash
docker compose up -d postgres
```

## Configuration

### Backend

```text
SPRING_PROFILES_ACTIVE=local
SERVER_PORT=8080

DB_HOST=localhost
DB_PORT=5432
DB_NAME=officeops
DB_USERNAME=officeops
DB_PASSWORD=officeops

JWT_SECRET=local-development-secret
JWT_ACCESS_TOKEN_EXPIRES_IN=3600000

CORS_ALLOWED_ORIGINS=http://localhost:5173
```

### Frontend

```text
VITE_API_BASE_URL=http://localhost:8080/api
```

운영 DB 비밀번호와 JWT Secret은 Git에 커밋하지 않습니다. 실제 구현 시 `.env.example`을 제공하고 `.env`는 `.gitignore`에 포함합니다.

## API Summary

| Domain | API |
| --- | --- |
| Auth/User Admin | `POST /api/admin/users`, `POST /api/auth/login`, `GET /api/users/me` |
| Requests | `GET /api/requests`, `POST /api/requests`, `GET /api/requests/{id}` |
| Manager | `GET /api/manager/approvals`, `PATCH /api/manager/approvals/{id}/approve` |
| Operator | `GET /api/operator/requests`, `PATCH /api/operator/requests/{id}/approve` |
| Reservations | `GET /api/resources`, `POST /api/reservations`, `PATCH /api/reservations/{id}/cancel` |
| Assets | `GET /api/assets`, `POST /api/admin/assets`, `POST /api/admin/assets/{id}/loans` |
| Dashboard | `GET /api/admin/dashboard/summary` |
| Audit | `GET /api/admin/audit-logs` |

상세 API는 [API 명세서](./docs/specifications/OfficeOps_hub_인터페이스명세서_v1.3.0.md)를 기준으로 합니다.

## Testing

### Backend

```bash
cd backend
./gradlew test
```

### Frontend

```bash
cd frontend
npm run test
npm run lint
```

필수 테스트 대상은 인증/인가, 요청 상태 전이, 예약 중복 방지, 자산 대여/반납, 감사 이력 저장입니다.

## Roadmap

### MVP

- 인증/권한
- 요청 등록/조회/수정/취소
- 팀장 1차 승인/반려
- 운영 담당자 최종 승인/반려
- 담당자 지정과 처리 중/완료
- 회의실 예약과 중복 방지
- 자산 관리와 대여/반납
- 대시보드와 감사 이력
- Swagger API 문서

### Future

- 요청 댓글/처리 코멘트
- 인앱 알림
- 예약 캘린더
- 내 자산 대여 현황
- 파일 첨부
- CSV 다운로드
- 반복 예약
- AWS 배포
- GitHub Actions CI/CD

## Portfolio Points

- 사내 운영 업무를 실제 업무 흐름에 가깝게 모델링
- 역할별 권한 분리와 백엔드 권한 재검증
- 요청 상태 전이 규칙과 잘못된 상태 변경 차단
- 예약 시간 충돌 방지 로직
- 자산 대여/반납 상태와 이력 추적
- 관리자 감사 이력 저장
- 검색, 필터, 페이지네이션, 대시보드
- Docker와 AWS 기반 배포 확장 가능성

## License

This project is licensed under the MIT License.
See [LICENSE](./LICENSE) for details.
